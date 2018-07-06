---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
title: 'Implantação de Web do ASP.NET usando o Visual Studio: Implantando uma atualização de código | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar (publicar) um ASP.NET web de aplicativo para aplicativos de Web do serviço de aplicativo do Azure ou para um provedor de hospedagem de terceiros, usin...
ms.author: aspnetcontent
ms.date: 02/15/2013
ms.assetid: c76dbc35-a914-4ee3-919c-4f4d1fa05104
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
msc.type: authoredcontent
ms.openlocfilehash: c058d4314ea0bed87e22c0e6b3fa09b89b2423d9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833349"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-code-update"></a>Implantação de Web do ASP.NET usando o Visual Studio: Implantando uma atualização de código
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto inicial](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Esta série de tutoriais mostra como implantar (publicar) um ASP.NET web application para aplicativos de Web do serviço de aplicativo do Azure ou para um provedor de hospedagem de terceiros, usando o Visual Studio 2012 ou Visual Studio 2010. Para obter informações sobre a série, consulte [o primeiro tutorial na série](introduction.md).


## <a name="overview"></a>Visão geral

Após a implantação inicial, continua seu trabalho de manutenção e desenvolvimento de seu site da web e pouco tempo você deseja implantar uma atualização. Este tutorial o guiará durante o processo de implantação de uma atualização para o código do aplicativo. A atualização que você implementa e implantar este tutorial não envolve uma alteração de banco de dados; Você verá o que é diferente sobre como implantar uma alteração de banco de dados no próximo tutorial.

Lembrete: Se você receber uma mensagem de erro ou se algo não funciona ao percorrer o tutorial, não se esqueça de verificar a [página de solução de problemas](troubleshooting.md).

## <a name="make-a-code-change"></a>Alterar um código

Como um exemplo simples de uma atualização para o seu aplicativo, você adicionará a **instrutores** página uma lista de cursos ministrados pelo instrutor selecionado.

Se você executar o **instrutores** página, você observará que há **selecione** links na grade, mas eles não fazem nada diferente de marca a linha cinza de turno em segundo plano.

![Página instrutores com seleção](deploying-a-code-update/_static/image1.png)

Agora você adicionará código que é executada quando o **selecionar** link é clicado e exibe uma lista de cursos ministrados pelo instrutor selecionado.

1. Na *Instructors.aspx*, adicione a seguinte marcação imediatamente após o **ErrorMessageLabel** `Label` controle:

    [!code-aspx[Main](deploying-a-code-update/samples/sample1.aspx)]
2. Execute a página e selecione um instrutor. Você ver uma lista de cursos ministrados por instrutor.

    ![Página instrutores com cursos ministrados](deploying-a-code-update/_static/image2.png)
3. Feche o navegador.

## <a name="deploy-the-code-update-to-the-test-environment"></a>Implante a atualização de código para o ambiente de teste

Antes de usar seus perfis de publicação para implantar em teste, preparação e produção, você precisa alterar as opções de publicação de banco de dados. Você não precisa executar os scripts de implantação de concessão e dados para o banco de dados de associação.

1. Abra o **Publicar Web** assistente clicando com botão direito no projeto ContosoUniversity e clicando em **publicar**.
2. Clique o **teste** perfil na **perfil** lista suspensa.
3. Clique o **configurações** guia.
4. Sob **DefaultConnection** na **bancos de dados** seção, desmarque o **Atualizar banco de dados** caixa de seleção.
5. Clique o **perfil** guia e, em seguida, clique no **preparo** perfil na **perfil** lista suspensa.
6. Quando for perguntado se você deseja salvar as alterações feitas para o **teste** de perfil, clique em **Sim**.
7. Fazer a mesma alteração no perfil de preparo.
8. Repita o processo para fazer a mesma alteração no perfil produção.
9. Fechar o **publicar na Web** assistente.

A implantação no ambiente de teste é agora uma simples questão de execução em um único clique publique novamente. Para tornar esse processo mais rápido, você pode usar o **publicação Web com um clique** barra de ferramentas.

1. No **modo de exibição** menu, escolha **barras de ferramentas** e, em seguida, selecione **publicação Web com um clique**.

    ![Selecting_One_Click_Publish_toolbar](deploying-a-code-update/_static/image3.png)
2. Na **Gerenciador de soluções**, selecione o projeto ContosoUniversity.
3. o **publicação Web com um clique em** barra de ferramentas, escolha o **teste** perfil de publicação e, em seguida, clique em **publicar na Web** (o ícone com setas apontando para a esquerda e direita).

    ![Web_One_Click_Publish_toolbar](deploying-a-code-update/_static/image4.png)
4. O Visual Studio implanta o aplicativo atualizado e o navegador é aberto automaticamente para a home page.
5. Execute a página instrutores e selecione um instrutor para verificar se a atualização foi implantada com êxito.

Você faria normalmente também fazer testes de regressão (ou seja, teste o restante do site para certificar-se de que a nova alteração não quebra nenhuma funcionalidade existente). Mas para este tutorial, você vai ignorar essa etapa e continuar para implantar a atualização para o preparo e produção.

Quando você reimplanta, implantação da Web determina automaticamente quais arquivos foram alterados e apenas copia os arquivos alterados para o servidor. Por padrão, implantação da Web usa datas alteradas por último em arquivos para determinar quais delas foram alterados. Alguns sistemas de controle do código-fonte alterar arquivo datas, mesma quando você não altere o conteúdo do arquivo. Nesse caso, você talvez queira configurar a implantação da Web para usar as somas de verificação para determinar quais arquivos foram alterados. Para obter mais informações, consulte [por que todos os meus arquivos reimplantados embora eu não alterá-los?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) nas perguntas Frequentes de implantação de ASP.NET.

## <a name="take-the-application-offline-during-deployment"></a>Colocar o aplicativo offline durante a implantação

Agora, a alteração que você está implantando é uma alteração simple em uma única página. Mas, às vezes, você pode implantar alterações maiores, ou você implantar alterações de código e o banco de dados e o site pode se comportar incorretamente se um usuário solicita uma página antes da implantação for concluída. Para impedir que os usuários acessem o site durante a implantação está em andamento, você pode usar um *app\_offline.htm* arquivo. Quando você colocar um arquivo chamado *app\_offline.htm* na pasta raiz do seu aplicativo, o IIS exibe automaticamente esse arquivo em vez de executar seu aplicativo. Então, para impedir o acesso durante a implantação, coloque *app\_offline.htm* na pasta raiz, execute o processo de implantação e, em seguida, remova *app\_offline.htm* depois bem-sucedida implantação.

Você pode configurar a implantação da Web para colocar automaticamente um padrão *app\_offline.htm* de arquivos no servidor quando ele começar a implantar e removê-lo quando ela for concluída. Para fazer tudo o que você precisa fazer é adicionar o seguinte elemento XML ao seu arquivo de perfil (. pubxml) de publicação:

[!code-xml[Main](deploying-a-code-update/samples/sample2.xml)]

Para este tutorial, você verá como criar e usar um personalizado *app\_offline.htm* arquivo.

Usando o *app\_offline.htm* no site de preparo não é necessária, porque você não tem usuários que acessam o site de preparo. Mas é uma boa prática usar preparo para testar tudo o que a maneira como você planeja implantar na produção.

### <a name="create-appofflinehtm"></a>Criar aplicativo\_offline.htm

1. Na **Gerenciador de soluções**, a solução com o botão direito e clique em **Add**e, em seguida, clique em **Novo Item**.
2. Criar uma **página HTML** denominado *app\_offline.htm* (excluir o último "l" no *. HTML* extensão Visual Studio cria por padrão).
3. Substitua a marcação de modelo com a seguinte marcação:

    [!code-html[Main](deploying-a-code-update/samples/sample3.html)]
4. Salve e feche o arquivo.

### <a name="copy-appofflinehtm-to-the-root-folder-of-the-web-site"></a>Aplicativo de cópia\_offline.htm para a pasta raiz do site da web

Você pode usar qualquer ferramenta FTP para copiar arquivos para o site da web. [O FileZilla](http://filezilla-project.org/) é uma ferramenta popular de FTP e é mostrada nas capturas de tela.

Para usar uma ferramenta FTP, você precisa de três itens: a URL de FTP, o nome de usuário e a senha.

A URL é mostrada na página do painel do site da web no Portal de gerenciamento do Azure, e o nome de usuário e senha para FTP podem ser encontradas na *. publishsettings* arquivo que você baixou anteriormente. As etapas a seguir mostram como obter esses valores.

1. No Portal de gerenciamento do Azure, clique em **Sites da Web** guia e, em seguida, clique em site da web de preparo.
2. Sobre o **Dashboard** página, role para baixo até encontrar o nome de host de FTP na **visão rápida** seção.

    ![Nome do host FTP](deploying-a-code-update/_static/image5.png)
3. Abra o preparo *. publishsettings* arquivo no bloco de notas ou outro editor de texto.
4. Encontre o `publishProfile` elemento para o perfil FTP.
5. Cópia de `userName` e `userPWD` valores.

    ![Credenciais de FTP](deploying-a-code-update/_static/image6.png)
6. Abra sua ferramenta FTP e faça logon no URL FTP.
7. Cópia *app\_offline.htm* da pasta de solução para o */site/wwwroot* pasta no site de preparo.

    ![Copiar app_offline](deploying-a-code-update/_static/image7.png)
8. Navegue até a URL do seu site de preparo. Você verá que o *app\_offline.htm* página agora é exibida em vez de sua home page.

    ![App_offline na janela do navegador](deploying-a-code-update/_static/image8.png)

Agora você está pronto para implantar no preparo.

## <a name="deploy-the-code-update-to-staging-and-production"></a>Implantar a atualização de código em produção e preparo

1. No **publicação Web com um clique em** barra de ferramentas, escolha o **preparo** perfil de publicação e, em seguida, clique em **publicar na Web**.

    Visual Studio implanta o aplicativo atualizado e abre o navegador para a home page do site. O *app\_offline.htm* arquivo é exibido. Antes de poder testar para verificar a implantação bem-sucedida, você deve remover o *app\_offline.htm* arquivo.
2. Retornar à sua ferramenta FTP e exclua **app\_offline.htm** do site de preparo.
3. No navegador, abra a página instrutores no local de preparo e selecione um instrutor para verificar se a atualização foi implantada com êxito.
4. Siga o mesmo procedimento para produção, como você fez preparo.

<a id="specificfiles"></a>

## <a name="reviewing-changes-and-deploying-specific-files"></a>Revisar as alterações e implantar arquivos específicos

Visual Studio 2012 também oferece a capacidade de implantar os arquivos individuais. Para um arquivo selecionado você pode exibir as diferenças entre a versão local e a versão implantada, implantar o arquivo para o ambiente de destino ou copie o arquivo do ambiente de destino para o projeto local. Nesta seção do tutorial, você verá como usar esses recursos.

### <a name="make-a-change-to-deploy"></a>Faça uma alteração para implantar

1. Abra *Content/Site.css*e localize o bloco para o `body` marca.
2. Altere o valor de `background-color` partir `#fff` para `darkblue`.

    [!code-css[Main](deploying-a-code-update/samples/sample4.css?highlight=2)]

### <a name="view-the-change-in-the-publish-preview-window"></a>Exiba a alteração na janela de visualização de publicação

Quando você usa o **Publicar Web** Assistente para publicar o projeto, você pode ver quais alterações serão publicadas por duas vezes no arquivo na **visualização** janela.

1. Clique com botão direito no projeto ContosoUniversity e clique em **publicar**.
2. Dos **perfil** lista suspensa, selecione o **teste** perfil de publicação.
3. Clique em **versão prévia**e, em seguida, clique em **iniciar visualização**.
4. No **versão prévia** painel, clique duas vezes em **CSS**.

    ![Clique duas vezes em CSS](deploying-a-code-update/_static/image9.png)

    O **visualizar alterações** caixa de diálogo mostra uma visualização das alterações que serão implantados.

    ![Visualizar alterações de CSS](deploying-a-code-update/_static/image10.png)

    Se você clicar duas vezes o *Web. config* arquivo, o **visualizar alterações** caixa de diálogo mostra o efeito de sua compilação transformações de configuração e transformações de perfil de publicação. Neste momento você não tiver feito tudo o que faria com que o *Web. config* arquivo no servidor para mudar, portanto, você espera não ver nenhuma alteração. No entanto, o **visualizar alterações** janela mostra duas alterações incorretamente. Parece que os dois elementos XML serão removidos. Esses elementos são adicionados pelo processo de publicação quando você seleciona **executar migrações do Code First na inicialização do aplicativo** para uma classe de contexto de Code First. A comparação é feita antes que o processo de publicação adiciona esses elementos, portanto, parece que estão sendo removidos embora eles não serão removidos. Esse erro será corrigido em uma versão futura.
5. Clique em **Fechar**.
6. Clique em **Publicar**.
7. Quando o navegador é aberto para a home page do site de teste, pressione CTRL + F5 para fazer com que uma atualização de disco rígida para ver o efeito da alteração do CSS.

    ![Efeito da alteração CSS](deploying-a-code-update/_static/image11.png)
8. Feche o navegador.

### <a name="publish-specific-files-from-solution-explorer"></a>Publicar arquivos específicos do Gerenciador de soluções

Suponha que você não o plano de fundo azul e quiser reverter para a cor original. Nesta seção você irá restaurar as configurações originais ao publicar um arquivo específico diretamente de **Gerenciador de soluções**.

1. Abra *Content/Site.css* e restaurar o `background-color` definir como `#fff`.
2. Na **Gerenciador de soluções**, clique com botão direito do *Content/Site.css* arquivo.

    O menu de contexto mostra que três opções de publicação.

    ![Publicar opções no Gerenciador de soluções](deploying-a-code-update/_static/image12.png)
3. Clique em **visualizar alterações feitas em CSS**.

    Uma janela é aberta para mostrar as diferenças entre o arquivo local e a versão no ambiente de destino.

    ![Diff-conteúdo/CSS](deploying-a-code-update/_static/image13.png)
4. Na **Gerenciador de soluções**, clique com botão direito **CSS** novamente e clique em **publicar CSS**.

    O **atividade de publicação de Web** janela mostra que o arquivo foi publicado.

    ![Janela de atividade de publicação da Web](deploying-a-code-update/_static/image14.png)
5. Abra um navegador para o `http://localhost/contosouniversity` URL e, em seguida, pressione CTRL + F5 para fazer com que um disco rígido atualizar para ver o efeito de CSS alterar.

    ![Home page com CSS normal](deploying-a-code-update/_static/image15.png)
6. Feche o navegador.

## <a name="summary"></a>Resumo

Agora você já viu várias maneiras de implantar uma atualização de aplicativo que não envolve uma alteração de banco de dados, e você já viu como visualizar as alterações para verificar se o que será atualizado é o que você espera. A página instrutores agora tem um **cursos ministrados** seção.

![Página instrutores com cursos ministrados](deploying-a-code-update/_static/image16.png)

O próximo tutorial mostra como implantar uma alteração de banco de dados: você adicionará um campo de data de nascimento para o banco de dados e para a página instrutores.

> [!div class="step-by-step"]
> [Anterior](deploying-to-production.md)
> [Próximo](deploying-a-database-update.md)
