---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent
title: "Configurando um servidor Web para Web (agente remoto) de publicação da implantação | Microsoft Docs"
author: jrjlee
description: "Este tópico descreve como configurar um servidor web de serviços de informações da Internet (IIS) para dar suporte à implantação usando a implantação de Web do IIS e publicação na web..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 239c7aa8-d09a-4d02-9c0e-6bd52be5f0d5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent
msc.type: authoredcontent
ms.openlocfilehash: 61e357198ffa4e93d35b7fa4619270da630547c6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="configuring-a-web-server-for-web-deploy-publishing-remote-agent"></a>Configurando um servidor Web para publicação (agente remoto) de implantação da Web
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve como configurar um servidor web de serviços de informações da Internet (IIS) para dar suporte à publicação na web e implantação usando o serviço de agente remoto de ferramenta de implantação da Web de IIS (implantação da Web).
> 
> Quando você trabalha com a implantação da Web 2.0 ou posterior, há três abordagens principais que você pode usar para obter os aplicativos ou sites em um servidor web. Você pode:
> 
> - Use o *Web implantar o serviço de agente remoto*. Essa abordagem requer menos configuração do servidor web, mas você precisa fornecer as credenciais de um administrador de servidor local para implantar qualquer coisa no servidor.
> - Use o *manipulador de implantação da Web*. Essa abordagem é muito mais complexa e exige mais esforço inicial para configurar o servidor web. No entanto, quando você usar essa abordagem, você pode configurar o IIS para permitir que os usuários não administradores executar a implantação. O manipulador de implantação da Web só está disponível no IIS versão 7 ou posterior.
> - Use *implantação offline*. Essa abordagem requer que o mínimo de configuração do servidor web, mas um administrador de servidor manualmente deve copiar o pacote da web no servidor e importe-o por meio do Gerenciador do IIS.
> 
> Para obter mais informações sobre os principais recursos, vantagens e desvantagens dessas abordagens, consulte [optar pela abordagem da direita para a implantação da Web](choosing-the-right-approach-to-web-deployment.md).


## <a name="is-the-web-deploy-remote-agent-the-right-approach-for-you"></a>É a Web implantar o agente remoto a abordagem certa para você?

Sim, se o usuário que vai implantar o conteúdo pode fornecer as credenciais de administrador no servidor de destino. Essa abordagem geralmente é desejável nesses tipos de cenários:

- Ambientes de desenvolvimento ou teste, onde o desenvolvedor tem controle total sobre o servidor web de destino e o servidor de banco de dados.
- Organizações menores em que um único usuário ou um pequeno grupo de usuários tem controle sobre o ciclo de vida do aplicativo inteiro.

Em muitas organizações maiores e principalmente para ambientes de preparo ou produção, geralmente não é realista para dar aos usuários direitos de administrador nos servidores web. No caso de servidores web hospedado, isso é pouco provável que seja o caso. Além disso, se você estiver planejando automatizar a implantação de um servidor de compilação, não convém usar credenciais de administrador para o processo de implantação. Nesses cenários, configurando os servidores de web para dar suporte à implantação usando o [Web implantar manipulador](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md) pode fornecer uma escolha mais satisfatória.

## <a name="task-overview"></a>Visão geral da tarefa

Este tópico descreve como configurar um servidor web de serviços de informações da Internet (IIS) 7.5 para aceitar e implantar pacotes de web de um computador remoto usando a abordagem de Web implantar o agente remoto. Você precisará:

- Instale o IIS 7.5 e a configuração recomendada do IIS 7.
- Instale a implantação da Web 2.1 ou posterior.
- Crie um site do IIS para hospedar o conteúdo implantado.
- Certifique-se de que o serviço de agente de implantação da Web está em execução.

Para hospedar a solução de exemplo especificamente, você também precisará:

- Instale o .NET Framework 4.0.
- Instale o ASP.NET MVC 3.

Neste tópico mostram como executar cada um desses procedimentos. As tarefas e instruções passo a passo neste tópico pressupõem que você está iniciando com uma compilação limpa de servidor executando o Windows Server 2008 R2. Antes de continuar, certifique-se de que:

- Windows Server 2008 R2 Service Pack 1 e todas as atualizações disponíveis são instaladas.
- O servidor está ingressado no domínio.
- O servidor tem um endereço IP estático.

> [!NOTE]
> Para obter mais informações sobre como adicionar computadores a um domínio, consulte [ingressando computadores no domínio e fazendo logon](https://technet.microsoft.com/en-us/library/cc725618(v=WS.10).aspx). Para obter mais informações sobre como configurar endereços IP estáticos, consulte [configurar um endereço IP estático](https://technet.microsoft.com/en-us/library/cc754203(v=ws.10).aspx). O serviço de agente remoto tem suporte pelo IIS 6 em diante e não exige que você ingresse em um domínio. No entanto, as etapas neste tutorial foram desenvolvidas e testadas no IIS 7.5 e procedimentos para outras versões podem variar.


## <a name="install-products-and-components"></a>Instalar produtos e componentes

Esta seção o orientará durante a instalação dos produtos necessários e componentes no servidor web. Antes de começar, uma prática recomendada é executar o Windows Update para garantir que seu servidor está totalmente atualizado.

Nesse caso, você precisa instalar essas coisas:

- **Configuração recomendada do IIS 7**. Isso permite que o **servidor Web (IIS)** função em seu servidor web e instala o conjunto de módulos do IIS e os componentes necessários para hospedar um aplicativo ASP.NET.
- **.NET framework 4.0**. Isso é necessário para executar aplicativos que foram criados com esta versão do .NET Framework.
- **Web Deployment Tool 2.1 ou posterior**. Isso instala a implantação da Web (e seu executável subjacente, MSDeploy.exe) em seu servidor. Como parte desse processo, ele instala e inicia o serviço de agente de implantação da Web. Esse serviço permite que você implante pacotes da web de um computador remoto.
- **O ASP.NET MVC 3**. Isso instala os assemblies que você precisa executar aplicativos MVC 3.

> [!NOTE]
> Este passo a passo descreve o uso do Web Platform Installer para instalar e configurar os componentes necessários. Embora você não precisa usar o Web Platform Installer, ele simplifica o processo de instalação automaticamente detectando dependências e garantindo que você obtenha sempre as versões mais recentes do produto. Para obter mais informações, consulte [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/?linkid=9805118).


**Para instalar os componentes e produtos necessários**

1. Baixe e instale o [Web Platform Installer](https://go.microsoft.com/?linkid=9805118).
2. Quando a instalação for concluída, o Web Platform Installer será iniciado automaticamente.

    > [!NOTE]
    > Agora você pode iniciar o Web Platform Installer a qualquer momento do **iniciar** menu. Para isso, o **iniciar** menu, clique em **todos os programas**e, em seguida, clique em **Microsoft Web Platform Installer**.
3. Na parte superior do **Web Platform Installer 3.0** janela, clique em **produtos**.
4. No lado esquerdo da janela, no painel de navegação, clique em **estruturas**.
5. No **Microsoft .NET Framework 4** de linha, se o .NET Framework já não estiver instalado, clique em **adicionar**.

    > [!NOTE]
    > Você já pode ter instalado o .NET Framework 4.0 através do Windows Update. Se um produto ou componente já está instalado, o Web Platform Installer indicará isso substituindo o **adicionar** botão com o texto **instalado**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image1.png)
6. No **ASP.NET MVC 3 (Visual Studio 2010)** de linha, clique em **adicionar**.
7. No painel de navegação, clique em **Server**.
8. No **configuração recomendada do IIS 7** de linha, clique em **adicionar**.
9. No **ferramenta de implantação da Web 2.1** de linha, clique em **adicionar**.
10. Clique em **Instalar**. O Web Platform Installer mostrará uma lista de produtos & #x 2014; juntamente com quaisquer dependências associadas & #x 2014; a serem instalados e solicitará que você aceite os termos de licença.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image2.png)
11. Leia os termos de licença e se você concordar com os termos, clique em **aceito**.
12. Quando a instalação for concluída, clique em **concluir**e, em seguida, feche o **Web Platform Installer 3.0** janela.

Se você instalou o .NET Framework 4.0 antes de instalar o IIS, você precisará executar o [ferramenta de registro ASP.NET IIS](https://msdn.microsoft.com/en-us/library/k6h9cz8h(v=VS.100).aspx) (aspnet\_regiis.exe) para registrar a versão mais recente do ASP.NET no IIS. Se você não fizer isso, você descobrirá que o IIS servirá conteúdo estático (como HTML) sem problemas, mas ele retornará **404.0 de erro HTTP – não encontrado** quando você tenta navegar até o conteúdo ASP.NET. Você pode usar este procedimento para verificar se o ASP.NET 4.0 está registrado.

**Para registrar o ASP.NET 4.0 com IIS**

1. Clique em **iniciar**e, em seguida, digite **Prompt de comando**.
2. Nos resultados da pesquisa, clique com botão direito **Prompt de comando**e, em seguida, clique em **executar como administrador**.
3. Na janela do Prompt de comando, navegue até o **%WINDIR%\Microsoft.NET\Framework\v4.0.30319** directory.
4. Digite o seguinte comando e pressione Enter:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-remote-agent/samples/sample1.cmd)]
5. Se você planeja hospedar aplicativos de 64 bits web a qualquer momento, você também deve registrar a versão de 64 bits do ASP.NET com o IIS. Para fazer isso, na janela do Prompt de comando, navegue até o **%WINDIR%\Microsoft.NET\Framework64\v4.0.30319** directory.
6. Digite o seguinte comando e pressione Enter:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-remote-agent/samples/sample2.cmd)]

Como uma prática recomendada, use o Windows Update novamente neste momento para baixar e instalar as atualizações disponíveis para os novos produtos e componentes que você instalou.

## <a name="configure-the-iis-website"></a>Configurar o site do IIS

Antes de implantar o conteúdo da web ao seu servidor, você precisa criar e configurar um site do IIS para hospedar o conteúdo. A implantação da Web só pode implantar pacotes da web para um site do IIS existente; ele não é possível criar o site para você. Em um nível alto, você precisará concluir essas tarefas:

- Crie uma pasta no sistema de arquivos para hospedar seu conteúdo.
- Criar um site do IIS para servir o conteúdo e associá-la com a pasta local.
- Conceder permissões de leitura para a identidade do pool de aplicativos na pasta local.

Embora não haja nada que o impeça de implantação de conteúdo para o site padrão no IIS, essa abordagem não é recomendada para algo diferente de cenários de teste ou demonstração. Para simular um ambiente de produção, você deve criar um novo site do IIS com as configurações específicas para os requisitos do seu aplicativo.

**Para criar e configurar um site do IIS**

1. No sistema de arquivos local, crie uma pasta para armazenar seu conteúdo (por exemplo, **C:\DemoSite**).
2. Sobre o **iniciar** , aponte para **ferramentas administrativas**e, em seguida, clique em **serviços de informações da Internet (IIS) Manager**.
3. No Gerenciador do IIS no **conexões** painel, expanda o nó do servidor (por exemplo, **TESTWEB1**).

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image3.png)
4. Clique com botão direito do **Sites** nó e, em seguida, clique **Adicionar Site**.
5. No **nome do Site** , digite um nome para o site do IIS (por exemplo, **DemoSite**).
6. No **caminho físico** caixa, digite (ou procure) o caminho para a pasta local (por exemplo, **C:\DemoSite**).
7. No **porta** , digite o número da porta na qual você deseja hospedar o site (por exemplo, **85**).

    > [!NOTE]
    > Os números de porta padrão são 80 para HTTP e 443 para HTTPS. No entanto, se você hospedar esse site na porta 80, você precisará parar o site padrão antes de poder acessar seu site.
8. Deixe o **nome de Host** caixa em branco, a menos que você deseja configurar um registro de sistema de nome de domínio (DNS) para o site e, em seguida, clique em **Okey**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image4.png)

    > [!NOTE]
    > Em um ambiente de produção, você provavelmente desejará hospedar seu site na porta 80 e configurar um cabeçalho de host, juntamente com os registros DNS correspondentes. Para obter mais informações sobre como configurar os cabeçalhos de host no IIS 7, consulte [configurar um cabeçalho de Host para um Site da Web (IIS 7)](https://technet.microsoft.com/en-us/library/cc753195(WS.10).aspx). Para obter mais informações sobre a função de servidor DNS no Windows Server 2008 R2, consulte [visão geral do servidor DNS](https://technet.microsoft.com/en-gb/library/cc770392.aspx) e [servidor DNS](https://technet.microsoft.com/en-us/windowsserver/dd448607).
9. No painel **Ações** , em **Editar Site**, clique em **Ligações**.
10. No **ligações de Site** caixa de diálogo, clique em **adicionar**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image5.png)
11. No **Adicionar associação do Site** caixa de diálogo, defina o **endereço IP** e **porta** para coincidir com a configuração de site existente.
12. No **nome de Host** , digite o nome do servidor web (por exemplo, **TESTWEB1**) e, em seguida, clique em **Okey**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image6.png)

    > [!NOTE]
    > A primeira associação do site permite que você acesse o site localmente usando o endereço IP e a porta ou `http://localhost:85`. A segunda associação de site permite que você acessar o site de outros computadores no domínio usando o nome do computador (por exemplo, http://testweb1:85).
13. No **ligações de Site** caixa de diálogo, clique em **fechar**.
14. No **conexões** painel, clique em **Pools de aplicativos**.
15. No **Pools de aplicativos** painel, clique no nome de seu pool de aplicativos e, em seguida, clique em **configurações básicas**. Por padrão, o nome do seu pool de aplicativos irão corresponder ao nome do seu site (por exemplo, **DemoSite**).
16. No **versão do .NET Framework** lista, selecione **v 4.0.30319 do .NET Framework**e, em seguida, clique em **Okey**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image7.png)

    > [!NOTE]
    > A solução de exemplo requer o .NET Framework 4.0. Isso não é um requisito para a implantação da Web em geral.

Em ordem para seu site para atender ao conteúdo, a identidade do pool de aplicativos deve ter permissões de leitura na pasta local que armazena o conteúdo. No IIS 7.5, pools de aplicativos são executados com uma identidade de pool de aplicativos exclusivo por padrão (em contraste com versões anteriores do IIS, em pools de aplicativos será executado normalmente usando a conta de serviço de rede). A identidade do pool de aplicativos não é uma conta de usuário e não aparece em qualquer lista de usuários ou grupos de & #x 2014; em vez disso, ele é criado dinamicamente quando o pool de aplicativos foi iniciado. Cada identidade de pool de aplicativos é adicionada ao local **IIS\_IUSRS** o grupo de segurança como itens ocultos.

Para conceder permissões para uma identidade de pool de aplicativos em um arquivo ou pasta, que você tem duas opções:

- Atribuir permissões para a identidade do pool de aplicativos diretamente, usando o formato **IIS AppPool\***[nome do pool de aplicativos] * (por exemplo, **IIS AppPool\DemoSite**).
- Atribuir permissões para o **IIS\_IUSRS** grupo.

A abordagem mais comum é atribuir permissões ao local **IIS\_IUSRS** grupo porque essa abordagem permite que você altere os pools de aplicativos sem precisar reconfigurar permissões do sistema de arquivos. O procedimento a seguir usa essa abordagem baseada em grupo.

> [!NOTE]
> Para obter mais informações sobre identidades de pool de aplicativos no IIS 7.5, consulte [identidades do Pool de aplicativos](https://go.microsoft.com/?linkid=9805123).


**Para configurar permissões de pasta para um site do IIS**

1. No Windows Explorer, navegue até o local da pasta local.
2. Clique na pasta e, em seguida, clique em **propriedades**.
3. Sobre o **segurança** , clique em **editar**e, em seguida, clique em **adicionar**.
4. Clique em **locais**. No **locais** caixa de diálogo, selecione o servidor local e, em seguida, clique em **Okey**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image8.png)
5. No **selecionar usuários ou grupos** caixa de diálogo, digite **IIS\_IUSRS**, clique em **verificar nomes**e, em seguida, clique em **Okey**.
6. No **permissões para***[nome da pasta]*caixa de diálogo, observe que o novo grupo foi atribuído a **leitura &amp; executar**, **Listar pasta conteúdo**, e **leitura** permissões por padrão. Deixe inalterados e clique em **Okey**.
7. Clique em **Okey** para fechar o *[nome da pasta]***propriedades** caixa de diálogo.

Como uma tarefa final antes de tentar implantar todos os pacotes da web em seu servidor, você deve garantir que o serviço de agente de implantação da Web está em execução. Quando você implanta um pacote de um computador remoto, o serviço de agente de implantação da Web é responsável para extrair e instalar o conteúdo do pacote. O serviço é iniciado por padrão quando você instala a ferramenta de implantação da Web e é executado sob a identidade do serviço de rede.

Você pode verificar se um serviço está em execução de várias maneiras diferentes, usando vários utilitários de linha de comando ou cmdlets do Windows PowerShell. Este procedimento descreve uma abordagem simples com base em interface do usuário.

**Para verificar se o serviço de agente de implantação da Web está em execução**

1. No menu **Iniciar** , aponte para **Ferramentas Administrativas**e depois clique em **Serviços**.
2. Localize o **serviço de agente de implantação da Web** de linha e verifique o **Status** é definido como **iniciado**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image9.png)
3. Se o serviço não tenha sido iniciado, clique em **iniciar**.

## <a name="configure-firewall-exceptions"></a>Configurar exceções do Firewall

Por padrão, o serviço de agente remoto escuta na porta TCP 80, nesta URL:

http:// [*nome do servidor*] / MSDEPLOYAGENTSERVICE

Na maioria dos casos, você não precisará configurar as regras de firewall adicionais para o serviço de agente remoto, como servidores web geralmente escutam solicitações HTTP na porta 80. Se você personalizou a instalação para escutar em uma porta não padrão, você precisará configurar as exceções de firewall, conforme necessário.

## <a name="conclusion"></a>Conclusão

Neste ponto, seu servidor web está pronto para aceitar e instalar pacotes da web de um computador remoto. Antes de tentar implantar um aplicativo web para o servidor, convém verificar estes pontos principais:

- Você registrou o ASP.NET 4.0 com IIS?
- A identidade do pool de aplicativos tem acesso de leitura para a pasta de origem para o seu site?
- O serviço de agente de implantação da Web está em execução?

## <a name="further-reading"></a>Leitura adicional

Para obter orientação sobre como configurar arquivos de projeto Microsoft Build Engine (MSBuild) personalizados para implantar pacotes da web para o serviço de agente remoto, consulte [configurando propriedades de implantação de um ambiente de destino](configuring-deployment-properties-for-a-target-environment.md).

>[!div class="step-by-step"]
[Anterior](scenario-configuring-a-production-environment-for-web-deployment.md)
[Próximo](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
