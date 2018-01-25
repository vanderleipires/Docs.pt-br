---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
title: Single Sign-On (Criando aplicativos de nuvem do mundo Real com o Azure) | Microsoft Docs
author: MikeWasson
description: "Os aplicativos de nuvem criando Real World com livro eletrônico do Azure baseia-se em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas recomendadas que ele..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 7d82d5e9-0619-4f22-9e03-32a6d52940a5
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
msc.type: authoredcontent
ms.openlocfilehash: b3640c94a8ae9ede330c0fe6a392acb5843cb65c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="single-sign-on-building-real-world-cloud-apps-with-azure"></a>Logon único (Criando aplicativos de nuvem do mundo Real com o Azure)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download corrigi-lo projeto](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [baixar livro eletrônico](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> O **nuvem aplicativos do mundo Real criando com o Azure** livro eletrônico baseia-se em uma apresentação desenvolvida por Scott Guthrie. Explica as 13 padrões e práticas que podem ajudá-lo a ser bem-sucedido de desenvolvimento de aplicativos da web para a nuvem. Para obter informações sobre o livro eletrônico, consulte [primeiro capítulo](introduction.md).


Há muitos problemas de segurança de pensar sobre quando você estiver desenvolvendo um aplicativo de nuvem, mas para esta série nos concentraremos em um: o logon único. Pessoas pergunta frequentemente perguntam é: "Estou principalmente criando aplicativos para os funcionários da minha empresa; como hospedar esses aplicativos na nuvem e ainda permitir que eles usam o mesmo modelo de segurança que Meus funcionários conhece e usam no ambiente local quando estiver executando aplicativos que são hospedados dentro do firewall?" Uma das maneiras que podemos habilitar este cenário é chamada do Azure Active Directory (AD do Azure). AD do Azure permite que você disponibilizar enterprise linha de negócios (LOB) aplicativos pela Internet, e permite que você disponibilizar esses aplicativos para parceiros de negócios.

## <a name="introduction-to-azure-ad"></a>Introdução ao AD do Azure

[O AD do Azure](https://docs.microsoft.com/azure/active-directory/) fornece [do Active Directory](https://msdn.microsoft.com/library/windows/desktop/aa746492.aspx) na nuvem. Os principais recursos incluem o seguinte:

- Ele se integra ao Active Directory no local.
- Ele permite o logon único com seus aplicativos.
- Ele oferece suporte a padrões abertos como [SAML](http://en.wikipedia.org/wiki/SAML_2.0), [WS-Fed](http://en.wikipedia.org/wiki/WS-Federation), e [OAuth 2.0](http://oauth.net/2/).
- Ele dá suporte a Enterprise [Graph REST API](https://msdn.microsoft.com/library/hh974476.aspx).

Suponha que você tenha um ambiente do Active Directory do Windows Server no local que você usa para habilitar os funcionários entrar aplicativos de Intranet:

![](single-sign-on/_static/image1.png)

O que o AD do Azure permite fazer é criar um diretório na nuvem. É um recurso gratuito e fácil de configurar.

Ele pode ser totalmente independente do seu local no Active Directory. Você pode colocar qualquer pessoa que você deseja nele e autenticá-los em aplicativos da Internet.

![Windows Azure Active Directory](single-sign-on/_static/image2.png)

Ou você pode integrar suas instalações AD.

![AD e a integração do WAAD](single-sign-on/_static/image3.png)

Agora todos os funcionários que podem autenticar local também podem autenticar através da Internet – sem a necessidade de abrir um firewall ou implantar novos servidores no seu data center. Você pode continuar a utilizar todos os o ambiente existente do Active Directory que você conhece e usa atualmente para fornecer seus aplicativos internos logon único no recurso.

Depois que você fez essa conexão entre o AD e o AD do Azure, você também pode habilitar seus aplicativos web e seus dispositivos móveis para autenticar os funcionários na nuvem e você pode habilitar aplicativos de terceiros, como o Office 365, o SalesForce.com ou o Google apps, para aceitar o credenciais de funcionários. Se você estiver usando o Office 365, você está já configurado com o AD do Azure como o Office 365 usa o AD do Azure para autenticação e autorização.

![3º aplicativos de terceiros](single-sign-on/_static/image4.png)

A vantagem dessa abordagem é que, quando sua organização adiciona ou exclui um usuário ou um usuário altera uma senha, use o mesmo processo que você usa atualmente no seu ambiente local. Todos os das suas instalações AD alterações são propagadas automaticamente para o ambiente de nuvem.

Se sua empresa está usando ou mover para o Office 365, a boa notícia é que você terá de configurar automaticamente como o Office 365 usa o AD do Azure para autenticação do AD do Azure. Portanto, você pode usar facilmente em seus próprios aplicativos de autenticação mesmo que usa o Office 365.

## <a name="set-up-an-azure-ad-tenant"></a>Configurar um locatário do AD do Azure

um diretório do AD do Azure é chamado um AD do Azure [locatário](https://technet.microsoft.com/library/jj573650.aspx), e é muito fácil configurar um locatário. Vamos mostrar como é feito no Portal de gerenciamento do Azure para ilustrar os conceitos, mas claro como as outras funções portais você também pode fazê-lo usando um script ou uma API de gerenciamento.

No portal de gerenciamento, clique na guia do Active Directory.

![WAAD no portal](single-sign-on/_static/image5.png)

Você automaticamente tem um locatário do AD do Azure para sua conta do Azure, e você pode clicar no **adicionar** botão na parte inferior da página para criar diretórios adicionais. Talvez você queira um para um ambiente de teste e outro para produção, por exemplo. Pense cuidadosamente sobre o que você nomear um novo diretório. Se você usar seu nome para o diretório e, em seguida, você pode usar o nome novamente para um dos usuários, que podem ser confusas.

![Adicionar diretório](single-sign-on/_static/image6.png)

O portal tem suporte completo para criar, excluir e gerenciar os usuários nesse ambiente. Por exemplo, para adicionar um usuário acesse o **usuários** guia e clique no **adicionar usuário** botão.

![Botão Adicionar usuário](single-sign-on/_static/image7.png)

![Adicionar caixa de diálogo do usuário](single-sign-on/_static/image8.png)

Você pode criar um novo usuário que existe somente nesse diretório, ou você pode registrar uma Account da Microsoft como um usuário nesse diretório, ou o registro ou um usuário de outro diretório do AD do Azure como um usuário neste diretório. (Em um diretório real, o domínio padrão seria ContosoTest.onmicrosoft.com. Você também pode usar um domínio de sua escolha, como contoso.com.)

![Tipos de usuário](single-sign-on/_static/image9.png)

![Adicionar caixa de diálogo do usuário](single-sign-on/_static/image10.png)

Você pode atribuir o usuário a uma função.

![Perfil de usuário](single-sign-on/_static/image11.png)

E a conta é criada com uma senha temporária.

![Senha temporária](single-sign-on/_static/image12.png)

Os usuários que você criar dessa maneira imediatamente podem fazer logon para seus aplicativos web usando este diretório na nuvem.

O que é excelente de enterprise single sign-on, no entanto, é o **integração de diretórios** guia:

![Guia de integração de diretório](single-sign-on/_static/image13.png)

Se você habilitar a integração de diretório e [Baixe uma ferramenta](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx), você pode sincronizar esse diretório na nuvem com o existente no Active Directory local que você já estiver usando dentro da sua organização. Em seguida, todos os usuários armazenados em seu diretório aparecerá nesse diretório de nuvem. Seus aplicativos de nuvem agora podem se autenticar todos os seus funcionários usando suas credenciais do Active Directory existentes. E tudo isso é livre – a ferramenta de sincronização e o Azure AD em si.

A ferramenta é um assistente que é fácil de usar, como você pode ver essas capturas de tela. Eles não são instruções completas, apenas um exemplo mostrando o processo básico. Para obter mais informações de how-a--it, consulte os links a [recursos](#resources) seção no final deste capítulo.

![Assistente de configuração da ferramenta de sincronização de WAAD](single-sign-on/_static/image14.png)

Clique em **próximo**e, em seguida, insira suas credenciais do Active Directory do Azure.

![Assistente de configuração da ferramenta de sincronização de WAAD](single-sign-on/_static/image15.png)

Clique em **próximo**e, em seguida, insira seu local as credenciais do AD.

![Assistente de configuração da ferramenta de sincronização de WAAD](single-sign-on/_static/image16.png)

Clique em **próximo**e, em seguida, indique se você deseja armazenar um hash de suas senhas do AD na nuvem.

![Assistente de configuração da ferramenta de sincronização de WAAD](single-sign-on/_static/image17.png)

O hash de senha que você pode armazenar na nuvem é um hash unidirecional; as senhas reais nunca são armazenadas no AD do Azure. Se você decidir armazenar hashes na nuvem, você terá que usar [os serviços de Federação do Active Directory](https://technet.microsoft.com/library/hh831502.aspx) (ADFS). Também há [outros fatores a considerar ao escolher se deseja ou não usar o ADFS](https://technet.microsoft.com/library/jj573653.aspx). A opção AD FS requer algumas etapas de configuração adicional.

Se você optar por armazenar hashes na nuvem, terminar e a ferramenta inicia a sincronização de diretórios quando você clica em **próximo**.

![Assistente de configuração da ferramenta de sincronização de WAAD](single-sign-on/_static/image18.png)

E, em alguns minutos terminar.

![Assistente de configuração da ferramenta de sincronização de WAAD](single-sign-on/_static/image19.png)

Você só precisa executar isso em um controlador de domínio da empresa, no Windows 2003 ou superior. E nenhuma reinicialização é necessária. Quando terminar, todos os seus usuários estão na nuvem e você pode fazer logon único de qualquer web ou aplicativo móvel, usando SAML, OAuth ou WS-Fed.

Às vezes, fazemos sobre o nível de segurança, isso é – Microsoft usá-lo para seus próprios dados confidenciais da empresa? E a resposta for Sim, que podemos fazer. Por exemplo, se você for para o site do Microsoft SharePoint interno no [https://microsoft.sharepoint.com/](https://microsoft.sharepoint.com/), for solicitado a fazer logon.

![Entrar no Office 365](single-sign-on/_static/image20.png)

Microsoft habilitou o ADFS, portanto, quando você inserir uma ID da Microsoft, você é redirecionado para uma página de logon do ADFS.

![Entrada do AD FS](single-sign-on/_static/image21.png)

E, depois que você insere as credenciais armazenadas em uma conta interna do AD do Microsoft, você tem acesso a esse aplicativo interno.

![Site do SharePoint MS](single-sign-on/_static/image22.png)

Estamos usando um servidor de entrada do AD principalmente porque já tínhamos ADFS configurar antes do AD do Azure se tornou disponível, mas o processo de log é feito por meio de um diretório do AD do Azure na nuvem. Podemos colocar nossos documentos importantes, controle de origem, arquivos de gerenciamento de desempenho, relatórios de vendas e muito mais na nuvem e está usando essa mesma solução exata para protegê-los.

## <a name="create-an-aspnet-app-that-uses-azure-ad-for-single-sign-on"></a>Criar um aplicativo ASP.NET que usa o AD do Azure para logon único

Visual Studio torna fácil criar um aplicativo que usa o AD do Azure para logon único, como você pode ver algumas capturas de tela.

Quando você cria um novo aplicativo ASP.NET, MVC ou Web Forms, o método de autenticação padrão é a identidade do ASP.NET. Para alterá-la para o AD do Azure, clique em uma **alterar autenticação** botão.

![Alterar autenticação](single-sign-on/_static/image23.png)

Selecione as contas organizacionais, insira seu nome de domínio e, em seguida, selecionar o logon único.

![Configurar a caixa de diálogo de autenticação](single-sign-on/_static/image24.png)

Também pode dar a aplicativo de leitura ou leitura/gravação permissão para dados de diretório. Se você fizer isso, ele pode usar o [Azure Graph REST API](https://msdn.microsoft.com/library/windowsazure/hh974476.aspx) para consultar o número de telefone dos usuários, descubra se estiverem no escritório, quando o último logon, etc.

Isso é tudo o que você precisa fazer - Visual Studio solicitará as credenciais de um administrador para seu locatário do AD do Azure e, em seguida, ele configura seu projeto e o seu locatário do AD do Azure para o novo aplicativo.

Quando você executar o projeto, você verá uma página de entrada, e você pode entrar com credenciais de um usuário em seu diretório do AD do Azure.

![Org conta entrar](single-sign-on/_static/image25.png)

![Conectado](single-sign-on/_static/image26.png)

Quando você implanta o aplicativo no Azure, tudo o que você precisa fazer é selecionar um **habilitar a autenticação organizacional** caixa de seleção e novamente o Visual Studio cuida da configuração para você.

![Publicar na Web](single-sign-on/_static/image27.png)

Essas capturas de tela vêm de um tutorial passo a passo completo que mostra como criar um aplicativo que usa a autenticação do AD do Azure: [desenvolvendo aplicativos do ASP.NET com o Azure Active Directory](../../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md).

## <a name="summary"></a>Resumo

Neste capítulo, você viu que Active Directory do Azure, o Visual Studio e o ASP.NET, mais fácil configurar o logon único em aplicativos da Internet para os usuários da organização. Seus usuários podem inscrever-se em aplicativos da Internet usando as mesmas credenciais que eles usam para fazer logon usando o Active Directory na sua rede interna.

O [próximo capítulo](data-storage-options.md) examina as opções de armazenamento de dados disponíveis para um aplicativo de nuvem.

<a id="resources"></a>
## <a name="resources"></a>Recursos

Para obter mais informações, consulte os seguintes recursos:

- [Documentação do Active Directory do Azure](https://docs.microsoft.com/azure/active-directory/). Página do portal para obter a documentação do AD do Azure no site windowsazure.com. Para obter os tutoriais passo a passo, consulte o **desenvolver** seção.
- [Autenticação multifator do Azure](https://docs.microsoft.com/azure/multi-factor-authentication/). Página do portal para documentação sobre a autenticação multifator no Azure.
- [Opções de autenticação de conta organizacional](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauthoptions). Explicação das opções de autenticação do AD do Azure, na caixa de diálogo de novo projeto Visual Studio 2013.
- [Padrões e práticas - Microsoft federada padrão de identidade](https://msdn.microsoft.com/library/dn589790.aspx).
- [Como: Instalar a ferramenta de sincronização do Active Directory do Azure](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx).
- [Serviços de Federação do Active Directory 2.0 mapa de conteúdo](https://social.technet.microsoft.com/wiki/contents/articles/2735.ad-fs-2-0-content-map.aspx). Links para documentação sobre o AD FS 2.0.
- [Autorização baseada em função e baseada em ACL em um aplicativo do Windows Azure AD](https://code.msdn.microsoft.com/Role-Based-and-ACL-Based-86ad71a1). Aplicativo de exemplo.
- [Blog do Azure Active Directory Graph API](https://blogs.msdn.com/b/aadgraphteam/).
- [Controle de acesso no BYOD e integração de diretórios em uma infraestrutura de identidade híbrida](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/PCIT-B213#fbid=). Tech Ed 2014 sessão vídeo por Gayana Bagdasaryan.

>[!div class="step-by-step"]
[Anterior](web-development-best-practices.md)
[Próximo](data-storage-options.md)
