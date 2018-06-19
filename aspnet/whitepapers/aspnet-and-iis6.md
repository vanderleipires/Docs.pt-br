---
uid: whitepapers/aspnet-and-iis6
title: Executando o ASP.NET 1.1 com o IIS 6.0 | Microsoft Docs
author: rick-anderson
description: Enquanto o Windows Server 2003 inclui o IIS 6.0 e ASP.NET 1.1, esses componentes são desabilitados por padrão. Este white paper descreve como habilitar o IIS 6.0 um...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 5a5537bf-2aaa-49e7-839f-9e6522b829d8
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-and-iis6
msc.type: content
ms.openlocfilehash: 1fcac7b8bc295ccf4e36189295b6bc2e4d328623
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26530105"
---
<a name="running-aspnet-11-with-iis-60"></a>Executando o ASP.NET 1.1 com o IIS 6.0
====================
> Enquanto o Windows Server 2003 inclui o IIS 6.0 e ASP.NET 1.1, esses componentes são desabilitados por padrão. Este white paper descreve como habilitar o IIS 6.0 e ASP.NET 1.1 e recomenda várias definições de configuração para obter o desempenho ideal do IIS e ASP.NET.
> 
> Aplica-se a ASP.NET 1.1 e o IIS 6.0.


ASP.NET 1.1 é fornecido com o Windows Server 2003, que também inclui a versão mais recente do Internet Information Server (IIS) versão 6.0. IIS 6.0 e ASP.NET 1.1 são projetados para integrar-se perfeitamente e o ASP.NET padrão agora é o novo modelo de processo de trabalho do IIS 6.0.

## <a name="aspnet-11-is-not-installed-by-default"></a>ASP.NET 1.1 não está instalado por padrão

Diferentemente das versões anteriores dos sistemas operacionais da Microsoft, o servidor de informações da Internet (IIS) não está habilitado por padrão. nem ASP.NET 1.1. Há duas opções para habilitar o IIS:

### <a name="enabling-iis-option-1---configure-your-server-wizard"></a>Habilitar o IIS, a opção #1 - o Assistente para configurar o servidor

Windows Server 2003 fornece um novo 'Assistente' para ajudá-lo a configurar adequadamente o servidor no modo desejado.

Para iniciar o assistente - Observe que, para executar o assistente, você deve ser conectado como um administrador - acesse: Iniciar | Programas | Ferramentas administrativas e selecione 'Configurar o servidor'.

Uma vez selecionada, você deverá ver a tela de abertura 'Assistente':

![](aspnet-and-iis6/_static/image1.jpg)

Clique em ' Avançar &gt;':

![](aspnet-and-iis6/_static/image2.jpg)

Clique em ' Avançar &gt;'

![](aspnet-and-iis6/_static/image3.jpg)

Nessa tela, será necessário selecionar ' servidor de aplicativos (IIS, ASP.NET) como as opções para configurar.

Clique em ' Avançar &gt;'.

![](aspnet-and-iis6/_static/image4.jpg)

Depois de selecionar esta opção para configurar o servidor como um servidor de aplicativos, a tela será exibida solicitando que recursos adicionais devem ser instalados. Nenhuma opção é selecionada por padrão. Para habilitar o ASP.NET automaticamente, você precisa selecionar ' Habilitar ASP. NET'.

Clique em ' Avançar &gt;'.

![](aspnet-and-iis6/_static/image5.jpg)

Essa tela exibe quais opções estão a ser instalado.

Clique em ' Avançar &gt;'.

![](aspnet-and-iis6/_static/image6.jpg)

Você verá esta tela enquanto as opções que você selecionou estão sendo instaladas. É normal para ver outros diálogo caixas aparecem como serviços estão sendo instalados. Além disso, você pode solicitado para o local do CD de instalação do Windows Server 2003.

Clique em ' Avançar &gt;' quando concluir.

![](aspnet-and-iis6/_static/image7.jpg)

Clique em 'Concluir' - o Windows Server 2003 está configurado para dar suporte a IIS 6.0 e ASP.NET 1.1.

### <a name="enabling-iis-option-2---manually-configuring-iis-and-aspnet"></a>Habilitar o IIS, a opção #2 - configurar manualmente o IIS e ASP.NET

Se você não quiser usar o 'Assistente' opcionalmente você pode instalar o IIS 6.0 e ASP.NET 1.1 usando 'Adicionar ou remover programas' no painel de controle.

Primeiro, abra o painel de controle:

![](aspnet-and-iis6/_static/image8.jpg)

Em seguida, clique em 'Adicionar ou remover componentes do Windows' que abrirá o 'Assistente de componentes do Windows':

![](aspnet-and-iis6/_static/image9.jpg)

Realce e verifique o 'servidor de aplicativos ' e, em seguida, clique em "Detalhes"? botão:

![](aspnet-and-iis6/_static/image10.jpg)

Para instalar o ASP.NET, verifique ' ASP. NET'.

Clique em 'Okey' para retornar ao Assistente de componentes do Windows. Clique em ' Avançar &gt;' do Assistente de componentes do Windows para começar a instalação:

![](aspnet-and-iis6/_static/image11.jpg)

É normal para ver outros diálogo caixas aparecem como serviços estão sendo instalados. Além disso, você pode solicitado para o local do CD de instalação do Windows Server 2003.

Quando a instalação for concluída, você verá a última tela do Assistente de componentes do Windows:

![](aspnet-and-iis6/_static/image12.jpg)

IIS 6.0 e ASP.NET 1.1 estão configurados e disponíveis.

## <a name="recommended-settings"></a>Configurações recomendadas

Ao executar o ASP.NET 1.1 com o IIS 6.0, há várias configurações que são recomendadas para obter o desempenho ideal do ASP.NET:

- Configurar limites de memória de processo de trabalho
- Configurando a reciclagem do processo de trabalho

### <a name="configuring-worker-process-memory-limits"></a>Configurar limites de memória de processo de trabalho

Por padrão o IIS 6.0 não definir um limite na quantidade de memória que o IIS tem permissão para usar. AS PÁGINAS ASP. O recurso de Cache do NET se baseia em um limite de memória para que o Cache de forma proativa remover itens não usados da memória.

É recomendável que você configure a recurso do IIS 6.0 de reciclagem de memória. Para configurar este abrir Gerenciador de serviços de informações da Internet (Iniciar | Programas | Ferramentas administrativas | Serviços de informações da Internet). Quando aberto, expanda a pasta de 'Pools de aplicativos ':

Para cada pool de aplicativos:

![](aspnet-and-iis6/_static/image13.jpg)

1. Clique com botão direito no pool de aplicativos, por exemplo 'DefaultAppPool' e selecione 'Propriedades':

![](aspnet-and-iis6/_static/image14.jpg)

2. Em seguida, habilitar a reciclagem de memória clicando em um ' máximo de memória usada (em megabytes):'. O valor não deve ser maior que a quantidade de memória física de (não virtual) no servidor, uma boa aproximação é 60% da memória física, ou seja, para um servidor com 512MB de memória física, selecione 310. Também é recomendável que o máximo não exceda 800MB ao usar um espaço de endereço de 2GB. Se o espaço de endereço de memória do servidor é 3GB, o limite máximo de memória do processo de trabalho pode ser de até 1, 800MB:

![](aspnet-and-iis6/_static/image15.jpg)

Clique em 'Aplicar' e 'Okey' para sair da caixa de diálogo Propriedades. Repita essa etapa para todos os pools de aplicativos disponíveis.

### <a name="configuring-worker-recycling"></a>Configurando o trabalho de reciclagem

Por padrão, o IIS 6.0 é configurado para reciclar o processo de trabalho cada 29 horas. Este é um pouco agressivo para um aplicativo em execução ASP.NET e é recomendável que a reciclagem do processo de trabalho automático está desativado.

Para desativar a reciclagem de processo de trabalho automático, primeiro abra o Gerenciador de serviços de informações da Internet (Iniciar | Programas | Ferramentas administrativas | Serviços de informações da Internet). Quando aberto, expanda a pasta de 'Pools de aplicativos ':

![](aspnet-and-iis6/_static/image16.jpg)

Para cada pool de aplicativos:

1. Clique com botão direito no pool de aplicativos, por exemplo 'DefaultAppPool' e selecione 'Propriedades':

![](aspnet-and-iis6/_static/image17.jpg)

2. Desmarque a opção ' Reciclar processos do operador (em minutos):':

![](aspnet-and-iis6/_static/image18.jpg)

Clique em 'Aplicar' e 'Okey' para sair da caixa de diálogo Propriedades. Repita essa etapa para todos os pools de aplicativos disponíveis.

## <a name="granting-write-access-to-the-file-system"></a>Concedendo acesso de gravação para o sistema de arquivos

Se seu aplicativo requer acesso de gravação para o sistema de arquivos e está usando o NTFS você precisará modificar uma lista de controle de acesso (ACL) no arquivo ou pasta para conceder acesso ASP.NET.

Por exemplo, conceder ASP.NET acesso de gravação para o c:\inetpub\wwwroot primeiro abra o explorer e navegue até o diretório:

![](aspnet-and-iis6/_static/image19.jpg)

Em seguida, clique no diretório, por exemplo, 'wwwroot' e selecione Propriedades. Depois de abre a caixa de diálogo de propriedades, selecione a guia 'Segurança':

![](aspnet-and-iis6/_static/image20.jpg)

O diretório de c:\inetpub\wwwroot\ é um diretório especial em que o grupo especial do IIS 6.0 ' IIS\_WPG' já foi concedida a leitura &amp; permissões de execução, Listar conteúdo da pasta e leitura. No entanto, para conceder permissão de gravação, você precisa clicar na caixa de seleção Permitir para gravação:

![](aspnet-and-iis6/_static/image21.jpg)

Agora, o IIS 6.0 tem permissão de gravação nessa pasta. Para conceder permissões de gravação em outras pastas, execute as seguintes etapas - Observe que talvez seja necessário adicionar o IIS\_grupo WPG se ele ainda não existir.

> [!CAUTION]
> Conceder permissão de gravação para o IIS\_WPG permitirá que qualquer aplicativo ASP.NET gravar nesse diretório.

## <a name="supporting-integrated-authentication-with-sql-server"></a>Com suporte a autenticação integrada com o SQL Server

Permite a autenticação integrada do SQL Server aproveitar a autenticação do Windows NT para validar as contas de logon do SQL Server. Isso permite que o usuário ignore o processo de logon padrão do SQL Server. Com essa abordagem, um usuário de rede pode acessar um banco de dados do SQL Server sem fornecer uma senha ou a identificação de logon separadas, porque o SQL Server obtém as informações de usuário e senha do processo de segurança de rede do Windows NT.

Escolher a autenticação integrada para aplicativos ASP.NET é uma boa opção porque nenhuma credencial nunca é armazenadas na cadeia de caracteres de conexão para o seu aplicativo. Em vez disso, a cadeia de conexão usada para se conectar ao SQL será semelhante ao seguinte:

`"server=localhost; database=Northwind;Trusted_Connection=true"`

Essa cadeia de caracteres de conexão informa ao SQL Server para usar as credenciais do Windows do aplicativo tentar acessar o SQL Server. No caso do ASP.NET /IIS 6, isso seria uma conta no IIS\_grupo WPG.

Para habilitar a autenticação integrada entre o SQL Server e o ASP.NET, você precisará primeiro certifique-se de que o SQL Server está configurado para autenticação integrada ou a autenticação de modo misto - Verifique com seu DBA para determinar isso. Se o SQL Server estiver em um destes dois modos, você pode usar a autenticação integrada.

Abra o SQL Server Enterprise Manager (Iniciar | Programas | Microsoft SQL Server | Enterprise Manager), selecione o servidor apropriado e expanda a pasta de segurança:

![](aspnet-and-iis6/_static/image22.jpg)

Se ' BUILTINT\IIS\_WPG' grupo não estiver listado, clique em logons e selecione 'Novo logon':

![](aspnet-and-iis6/_static/image23.jpg)

No ' nome:' caixa de texto Digite ' \IIS [nome do domínio do servidor]\_WPG' ou clique no botão de reticências para abrir o selecionador de usuário/grupo do Windows NT:

![](aspnet-and-iis6/_static/image24.jpg)

Selecione IIS a máquina atual\_WPG grupo e clique em 'Adicionar' e Okey para fechar o seletor.

Em seguida, você precisa também definir o banco de dados padrão e as permissões para acessar o banco de dados. Para definir o banco de dados padrão escolha na lista suspensa lista, por exemplo, a Northwind abaixo é selecionado:

![](aspnet-and-iis6/_static/image25.jpg)

Em seguida, clique na guia acesso de banco de dados:

![](aspnet-and-iis6/_static/image26.jpg)

Clique na caixa de seleção para cada banco de dados que você deseja permitir o acesso à permissão. Você também precisará selecionar funções de banco de dados, a verificação de banco de dados\_proprietário garantirá que o logon tem todas as permissões necessárias para gerenciar e usar o banco de dados selecionado.

Clique em Okey para sair da caixa de diálogo de propriedade. Seu aplicativo ASP.NET agora está configurado para dar suporte à autenticação integrada do SQL Server.

## <a name="dont-run-aspnet-10-in-iis-60-native-mode"></a>ASP.NET 1.0 não são executados em modo nativo do IIS 6.0

Somente há suporte para o ASP.NET 1.0 no IIS 6.0 no modo de compatibilidade do IIS 5.

Para configurar o ASP.NET 1.0 para ser executado no modo de compatibilidade do IIS 5.0, abra o Gerenciador de serviços de Internet e Sites da Web de clique do botão direito e selecione Propriedades:

![](aspnet-and-iis6/_static/image27.jpg)

Alternar para a guia de serviço e verificar? Execute o serviço da WWW no modo de isolamento 5.0 do IIS?:

![](aspnet-and-iis6/_static/image28.jpg)
