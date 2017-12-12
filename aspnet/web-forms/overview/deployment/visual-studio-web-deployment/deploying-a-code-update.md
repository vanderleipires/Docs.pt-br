---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
title: "Implantação de Web do ASP.NET usando o Visual Studio: Implantando uma atualização de código | Microsoft Docs"
author: tdykstra
description: "Esta série de tutorial mostra como implantar (publicação) de uma ASP.NET web do aplicativo para aplicativos de Web do serviço de aplicativo do Azure ou para um provedor de hospedagem de terceiros, por usin..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: c76dbc35-a914-4ee3-919c-4f4d1fa05104
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
msc.type: authoredcontent
ms.openlocfilehash: 10da2b5013ae1348b69ea4f456d81bb4c4b73df6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-code-update"></a>Implantação de Web do ASP.NET usando o Visual Studio: Implantando uma atualização de código
====================
Por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto Starter](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Esta série de tutorial mostra como implantar (publicação) de uma ASP.NET web do aplicativo para aplicativos de Web do serviço de aplicativo do Azure ou para um provedor de hospedagem de terceiros, usando o Visual Studio 2012 ou Visual Studio 2010. Para obter informações sobre a série, consulte [primeiro tutorial na série](introduction.md).


## <a name="overview"></a>Visão Geral

Após a implantação inicial, o trabalho de manutenção e desenvolvimento de seu site continua e pouco tempo você deseja implantar uma atualização. Este tutorial leva você através do processo de implantação de uma atualização para o código do aplicativo. A atualização que você implementa e implantar este tutorial não envolve uma alteração de banco de dados; Você verá qual é a diferença sobre a implantação de uma alteração de banco de dados do tutorial Avançar.

Lembrete: Se você receber uma mensagem de erro ou algo não funciona ao percorrer o tutorial, certifique-se verificar a [página de solução de problemas](troubleshooting.md).

## <a name="make-a-code-change"></a>Alterar um código

Como um exemplo simples de uma atualização para o seu aplicativo, você adicionará o **instrutores** página uma lista de cursos ministrada pelo instrutor selecionado.

Se você executar o **instrutores** página, você observará que há **selecione** links na grade, mas eles não fazem nada diferente de tornar o cinza de ativar linha em segundo plano.

![Página seleção de instrutores](deploying-a-code-update/_static/image1.png)

Agora você adicionará código que é executado quando o **selecione** link é clicado e exibe uma lista de cursos ministrada pelo instrutor selecionado.

1. Em *Instructors.aspx*, adicione a seguinte marcação imediatamente após o **ErrorMessageLabel** `Label` controle:

    [!code-aspx[Main](deploying-a-code-update/samples/sample1.aspx)]
2. Execute a página e selecione um instrutor. Você ver uma lista de cursos ministrada por esse instrutor.

    ![Página de instrutores cursos ministrada](deploying-a-code-update/_static/image2.png)
3. Feche o navegador.

## <a name="deploy-the-code-update-to-the-test-environment"></a>Implante a atualização de código para o ambiente de teste

Antes de usar os perfis de publicação para implantar em teste, preparação e produção, você precisa alterar as opções de publicação de banco de dados. Você não precisa executar os scripts de implantação de conceder e dados para o banco de dados de associação.

1. Abra o **Publicar Web** assistente clicando duas vezes no projeto ContosoUniversity e clicando em **publicar**.
2. Clique o **teste** perfil no **perfil** lista suspensa.
3. Clique o **configurações** guia.
4. Em **DefaultConnection** no **bancos de dados** seção, desmarque o **Atualizar banco de dados** caixa de seleção.
5. Clique o **perfil** guia e, em seguida, clique no **preparo** perfil no **perfil** lista suspensa.
6. Quando for perguntado se você deseja salvar as alterações feitas a **teste** de perfil, clique em **Sim**.
7. Fazer a mesma alteração no perfil de preparo.
8. Repita o processo para fazer a mesma alteração no perfil de produção.
9. Fechar o **Publicar Web** assistente.

A implantação no ambiente de teste é agora uma simples questão de executar um clique publicar novamente. Para tornar esse processo mais rápido, você pode usar o **Web um clique em publicar** barra de ferramentas.

1. No **exibição** menu, escolha **barras de ferramentas** e, em seguida, selecione **Web um clique em publicar**.

    ![Selecting_One_Click_Publish_toolbar](deploying-a-code-update/_static/image3.png)
2. Em **Solution Explorer**, selecione o projeto ContosoUniversity.
3. o **Web um clique em publicar** barra de ferramentas, escolha o **teste** perfil de publicação e, em seguida, clique em **Publicar Web** (o ícone com setas que apontam para a esquerda e direita).

    ![Web_One_Click_Publish_toolbar](deploying-a-code-update/_static/image4.png)
4. O Visual Studio implanta o aplicativo atualizado e o navegador é aberto automaticamente para a home page.
5. Execute a página de professores e selecione um instrutor para verificar se a atualização foi implantada com êxito.

Normalmente também fazer o teste de regressão (ou seja, teste o restante do site para certificar-se de que a nova alteração não violam nenhuma funcionalidade existente). Mas, para este tutorial você vai ignorar essa etapa e vá para implantar a atualização em preparação e produção.

Quando você reimplanta, Web Deploy determina automaticamente quais arquivos foram alterados e somente cópias de arquivos alterados para o servidor. Por padrão, implantação da Web usa datas alterada por último em arquivos para determinar quais foram alterados. Alguns sistemas de controle de origem alterar arquivo datas mesmo quando você não alterar o conteúdo do arquivo. Nesse caso, você poderá configurar a implantação da Web para usar as somas de verificação para determinar quais arquivos foram alterados. Para obter mais informações, consulte [por que todos os meus arquivos obter reimplantados embora eu não alterá-los?](https://msdn.microsoft.com/en-us/library/ee942158.aspx#use_checksum) nas perguntas Frequentes de implantação de ASP.NET.

## <a name="take-the-application-offline-during-deployment"></a>Colocar o aplicativo offline durante a implantação

A alteração que você está implantando agora é uma alteração simple em uma única página. Mas, às vezes, você pode implantar alterações maiores, ou implantar as alterações de código e o banco de dados e o site pode se comportar incorretamente se um usuário solicita uma página antes da conclusão da implantação. Para impedir que os usuários acessem o site durante a implantação está em andamento, você pode usar um *aplicativo\_offline.htm* arquivo. Quando você coloca um arquivo chamado *aplicativo\_offline.htm* na pasta raiz do seu aplicativo, o IIS exibe automaticamente esse arquivo em vez de executar seu aplicativo. Portanto para impedir acesso durante a implantação, você deve colocar *aplicativo\_offline.htm* na pasta raiz, execute o processo de implantação e, em seguida, remover *aplicativo\_offline.htm* depois bem-sucedida implantação.

Você pode configurar a implantação da Web para colocar automaticamente um padrão *aplicativo\_offline.htm* de arquivos no servidor quando ele começar a implantar e removê-lo quando ele for concluído. Para fazer o que você precisa fazer é adicionar o seguinte elemento XML para o arquivo de perfil (. pubxml) de publicação:

[!code-xml[Main](deploying-a-code-update/samples/sample2.xml)]

Para este tutorial, você verá como criar e usar um personalizado *aplicativo\_offline.htm* arquivo.

Usando *aplicativo\_offline.htm* no site de preparo não é necessária, porque você não tem usuários que acessam o site de preparo. Mas é uma boa prática usar preparo para testar tudo o modo como você planeja implantar na produção.

### <a name="create-appofflinehtm"></a>Criar aplicativo\_offline.htm

1. Em **Solution Explorer**, clique com botão direito a solução e clique em **adicionar**e, em seguida, clique em **Novo Item**.
2. Criar um **página HTML** chamado *aplicativo\_offline.htm* (excluir o último "l" no *. HTML* extensão que o Visual Studio cria por padrão).
3. Substitua a marcação de modelo com a seguinte marcação:

    [!code-html[Main](deploying-a-code-update/samples/sample3.html)]
4. Salve e feche o arquivo.

### <a name="copy-appofflinehtm-to-the-root-folder-of-the-web-site"></a>Aplicativo de cópia\_offline.htm para a pasta raiz do site da web

Você pode usar qualquer ferramenta FTP para copiar arquivos para o site da web. [FileZilla](http://filezilla-project.org/) é uma ferramenta popular de FTP e é mostrado nas capturas de tela.

Para usar uma ferramenta FTP, é necessário que três coisas: a URL de FTP, o nome de usuário e a senha.

A URL é exibida na página do painel de controle do site da web no Portal de gerenciamento do Azure e o nome de usuário e senha de FTP podem ser encontrados na *. publishsettings* arquivo que você baixou anteriormente. As etapas a seguir mostram como obter esses valores.

1. No Portal de gerenciamento do Azure, clique em **Sites da Web** guia e, em seguida, clique no site de preparo.
2. Sobre o **painel** página, role para baixo até localizar o nome de host do FTP no **visão rápida** seção.

    ![Nome de host do FTP](deploying-a-code-update/_static/image5.png)
3. Abra a preparo *. publishsettings* arquivo no bloco de notas ou outro editor de texto.
4. Localizar o `publishProfile` elemento para o perfil de FTP.
5. Copie o `userName` e `userPWD` valores.

    ![Credenciais de FTP](deploying-a-code-update/_static/image6.png)
6. Abra sua ferramenta FTP e faça logon no URL FTP.
7. Cópia *aplicativo\_offline.htm* da pasta de solução para o */site/wwwroot* pasta no site de preparo.

    ![Copiar app_offline](deploying-a-code-update/_static/image7.png)
8. Navegue até a URL do site de preparo. Você verá que o *aplicativo\_offline.htm* página agora é exibida em vez de sua home page.

    ![App_offline.htm na janela do navegador](deploying-a-code-update/_static/image8.png)

Agora você está pronto para implantar em preparo.

## <a name="deploy-the-code-update-to-staging-and-production"></a>Implante a atualização de código para teste e produção

1. No **Web um clique em publicar** barra de ferramentas, escolha o **preparo** perfil de publicação e, em seguida, clique em **Publicar Web**.

    Visual Studio implanta o aplicativo atualizado e abre o navegador para a home page do site. O *aplicativo\_offline.htm* arquivo é exibido. Antes de você testar para verificar a implantação bem-sucedida, você deve remover o *aplicativo\_offline.htm* arquivo.
2. Voltar para a ferramenta de FTP e excluir **aplicativo\_offline.htm** do site de preparo.
3. No navegador, abra a página de professores no site de preparo e selecione um instrutor para verificar se a atualização foi implantada com êxito.
4. Siga o mesmo procedimento para produção quanto à preparação.

<a id="specificfiles"></a>

## <a name="reviewing-changes-and-deploying-specific-files"></a>Revisar as alterações e implantar arquivos específicos

O Visual Studio 2012 também fornece a capacidade de implantar arquivos individuais. Para o arquivo selecionado exibir diferenças entre a versão local e a versão implantada, implante o arquivo para o ambiente de destino ou copie o arquivo do ambiente de destino para o local do projeto. Nesta seção do tutorial, você verá como usar esses recursos.

### <a name="make-a-change-to-deploy"></a>Fazer uma alteração para implantar

1. Abra *Content/Site.css*e localize o bloco para o `body` marca.
2. Altere o valor de `background-color` de `#fff` para `darkblue`.

    [!code-css[Main](deploying-a-code-update/samples/sample4.css?highlight=2)]

### <a name="view-the-change-in-the-publish-preview-window"></a>Exiba a alteração na janela de visualização de publicação

Quando você usa o **Publicar Web** Assistente para publicar o projeto, você pode ver quais alterações serão publicadas clicando duas vezes no **visualização** janela.

1. Clique com botão direito no projeto ContosoUniversity e clique em **publicar**.
2. Do **perfil** lista suspensa, selecione o **teste** perfil de publicação.
3. Clique em **visualização**e, em seguida, clique em **visualização iniciar**.
4. No **visualização** painel, clique duas vezes em **Site.css**.

    ![Clique duas vezes no site](deploying-a-code-update/_static/image9.png)

    O **visualizar alterações** caixa de diálogo mostra uma visualização das alterações que serão implantados.

    ![Visualizar alterações para o site](deploying-a-code-update/_static/image10.png)

    Se você clicar duas vezes o *Web. config* arquivo, o **visualizar alterações** caixa de diálogo mostra o efeito de sua compilação transformações de configuração e transformações de perfil de publicação. Neste momento você não o fez tudo o que faria com que o *Web. config* arquivo no servidor para mudar, portanto você espera não ver nenhuma alteração. No entanto, o **visualizar alterações** janela mostra duas alterações incorretamente. Parece que dois elementos XML serão removidos. Esses elementos são adicionados pelo processo de publicação quando você seleciona **executar migrações do Code First no início do aplicativo** para uma classe de contexto Code First. A comparação é feita antes que o processo de publicação adiciona esses elementos, portanto parece que estão sendo removidos embora eles não serão removidos. Esse erro será corrigido em uma versão futura.
5. Clique em **Fechar**.
6. Clique em **Publicar**.
7. Quando o navegador abre a home page do site de teste, pressione CTRL + F5 para fazer com que uma atualização de disco rígida para ver o efeito da alteração de CSS.

    ![Efeito da alteração CSS](deploying-a-code-update/_static/image11.png)
8. Feche o navegador.

### <a name="publish-specific-files-from-solution-explorer"></a>Publicar arquivos específicos no Gerenciador de soluções

Suponha que você não deseja que o plano de fundo azul e deseja reverter para a cor original. Nesta seção, irá restaurar as configurações originais ao publicar um arquivo específico, diretamente do **Gerenciador de soluções**.

1. Abra *Content/Site.css* e restaurar o `background-color` definindo como `#fff`.
2. Em **Solution Explorer**, com o botão direito do *Content/Site.css* arquivo.

    O menu de contexto exibe que três opções de publicação.

    ![Publicar opções no Gerenciador de soluções](deploying-a-code-update/_static/image12.png)
3. Clique em **visualizar alterações feitas em Site.css**.

    Uma janela é aberta para mostrar as diferenças entre o arquivo local e a versão no ambiente de destino.

    ![Diff-conteúdo/Site.css](deploying-a-code-update/_static/image13.png)
4. Em **Solution Explorer**, clique com botão direito **Site.css** novamente e clique em **Site.css publicar**.

    O **atividade de publicar Web** janela mostra que o arquivo foi publicado.

    ![Janela de atividade de publicação da Web](deploying-a-code-update/_static/image14.png)
5. Abra um navegador para o `http://localhost/contosouniversity` URL e, em seguida, pressione CTRL + F5 para fazer com que um disco rígido atualizar para ver o efeito de CSS alterar.

    ![Home page com CSS normal](deploying-a-code-update/_static/image15.png)
6. Feche o navegador.

## <a name="summary"></a>Resumo

Você viu várias maneiras de implantar uma atualização de aplicativo que não envolve uma alteração de banco de dados, e você viu como visualizar as alterações para verificar se o que será atualizado é esperada. A página instrutores agora tem um **cursos ministrada** seção.

![Página de instrutores cursos ministrada](deploying-a-code-update/_static/image16.png)

O seguinte tutorial mostra como implementar uma alteração de banco de dados: você adicionará um campo de data de nascimento ao banco de dados e para a página de professores.

>[!div class="step-by-step"]
[Anterior](deploying-to-production.md)
[Próximo](deploying-a-database-update.md)
