---
uid: identity/overview/migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity
title: Migrando dados do provedor Universal para associações e perfis de usuário para a identidade do ASP.NET (c#) | Microsoft Docs
author: rustd
description: Este tutorial descreve as etapas necessárias migrar dados de função e de usuários e dados de perfil de usuário criados usando provedores universais de um aplicativo existente...
ms.author: riande
ms.date: 12/13/2013
ms.assetid: 2e260430-d13c-4658-bd05-e256fc0d63b8
msc.legacyurl: /identity/overview/migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 6115b2a6caca05659f1c35ce97954807a6fb01ae
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830840"
---
<a name="migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity-c"></a>Migrando dados do provedor Universal para associações e perfis de usuário para a identidade do ASP.NET (c#)
====================
por [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Robert McMurray](https://github.com/rmcmurray), [Suhas Joshi](https://github.com/suhasj)

> Este tutorial descreve as etapas necessárias migrar dados de perfil de usuário criados usando provedores universais de um aplicativo existente para o modelo de identidade do ASP.NET e de usuário e dados de função. A abordagem mencionada aqui migrar dados de perfil do usuário pode ser usados em um aplicativo com a associação do SQL também.


Com o lançamento do Visual Studio 2013, a equipe do ASP.NET introduziu um novo sistema ASP.NET Identity, e você pode ler mais sobre essa versão [aqui](../../index.md). Dando o artigo para migrar aplicativos web contra [associação de SQL para o novo sistema de identidade](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md), este artigo ilustra as etapas para migrar aplicativos existentes que seguem o modelo de provedores para o gerenciamento de usuário e a função para o novo modelo de identidade. O foco deste tutorial será principalmente sobre como migrar os dados de perfil do usuário para perfeitamente vinculá-lo no novo sistema. Migração de informações de usuário e a função é semelhante para a associação do SQL. A abordagem seguida para migrar dados de perfil pode ser usada em um aplicativo com a associação do SQL também.

Por exemplo, vamos começar com um aplicativo web criado usando o Visual Studio 2012 usa o modelo de provedores. Podemos será, em seguida, adicione código para gerenciamento de perfil, registrar um usuário, adicionar dados de perfil para os usuários, migrar o esquema de banco de dados e, em seguida, altere o aplicativo para usar o sistema de identidade para o gerenciamento de usuário e a função. Como um teste de migração, os usuários criados usando provedores Universal devem ser capazes de fazer logon e novos usuários devem ser capazes de se registrar.

> [!NOTE]
> Você pode encontrar o exemplo completo em [ https://github.com/suhasj/UniversalProviders-Identity-Migrations ](https://github.com/suhasj/UniversalProviders-Identity-Migrations).


## <a name="profile-data-migration-summary"></a>Resumo de migração de dados de perfil

Antes de iniciar com as migrações, vamos examine a experiência de armazenar dados de perfil no modelo de provedores. Dados de perfil para o aplicativo que os usuários podem ser armazenados de várias maneiras, mais comum entre eles sendo usando os provedores de perfis internos enviados junto com os provedores universais. Incluem as etapas

1. Adicione uma classe que tem as propriedades usadas para armazenar dados de perfil.
2. Adicione uma classe que estende 'ProfileBase' e implementa métodos para obter os dados de perfil acima para o usuário.
3. Habilitar o uso de provedores de perfil de padrão na *Web. config* de arquivo e definir a classe declarada na etapa 2 # a ser usado ao acessar informações de perfil.

As informações de perfil são armazenadas como dados binários na tabela 'Perfis' no banco de dados e de xml serializado.

Depois de migrar o aplicativo para usar o novo sistema ASP.NET Identity, as informações de perfil são desserializadas e armazenadas como propriedades da classe de usuário. Cada propriedade, em seguida, pode ser mapeada para colunas na tabela do usuário. A vantagem aqui é que as propriedades podem ser trabalhadas diretamente usando a classe de usuário, além de não precisar serializar/desserializar informações de dados cada vez quando ela é acessada.

## <a name="getting-started"></a>Guia de Introdução

1. Crie um novo aplicativo de Web Forms do ASP.NET 4.5 no Visual Studio 2012. A amostra atual usa o modelo de Web Forms, mas você pode usar o aplicativo MVC também.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image1.jpg)
2. Criar uma nova pasta de modelos para armazenar informações de perfil  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image1.png)
3. Como exemplo, vamos armazene a data de nascimento, cidade, altura e peso do usuário no perfil. A altura e peso são armazenados como uma classe personalizada chamada 'PersonalStats'. Para armazenar e recuperar o perfil, precisamos de uma classe que estende 'ProfileBase'. Vamos criar uma nova classe AppProfile para obter e armazenar informações de perfil.

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample1.cs)]
4. Habilitar o perfil na *Web. config* arquivo. Insira o nome da classe a ser usado para armazenar/recuperar informações do usuário criadas na etapa 3 do #.

    [!code-xml[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample2.xml)]
5. Adicione uma página web forms na pasta 'Conta' para obter os dados de perfil do usuário e armazená-lo. Clique com botão direito no projeto e selecione "Adicionar Novo Item". Adicione uma nova página de formulários da Web com a página mestra 'AddProfileData.aspx'. Copie o seguinte na seção 'MainContent':

    [!code-html[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample3.html)]

   Adicione o seguinte código no code-behind:

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample4.cs)]

   Adicione o namespace no qual AppProfile classe é definida para remover os erros de compilação.
6. Execute o aplicativo e criar um novo usuário com nome de usuário '**olduser'.** Navegue até a página 'AddProfileData' e adicione informações de perfil do usuário.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image2.png)

Você pode verificar que os dados são armazenados como xml serializado na tabela 'Perfis' usando a janela do Gerenciador de servidores. No Visual Studio, no menu 'Exibir', escolha 'Gerenciador de servidores'. Deve haver uma conexão de dados para o banco de dados definido na *Web. config* arquivo. Clicar na conexão de dados mostra diferentes subcategorias. Expanda as tabelas para mostrar as tabelas diferentes no banco de dados, em seguida, clique com botão direito em 'Perfis' e escolha 'Mostrar dados da tabela' para exibir os dados de perfil armazenados na tabela de perfis.

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image3.png)

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image4.png)

## <a name="migrating-database-schema"></a>Migrar o esquema de banco de dados

Para tornar o banco de dados existente para trabalhar com o sistema de identidade, precisamos atualizar o esquema no banco de dados de identidade para dar suporte os campos que adicionamos ao banco de dados original. Isso pode ser feito usando os scripts SQL para criar novas tabelas e copiar as informações existentes. Na janela do 'Gerenciador de servidores ', expanda 'DefaultConnection' para exibir as tabelas. Tabelas de clique com botão direito e selecione 'Nova consulta'

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image5.png)

Cole o script SQL a partir [ https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt ](https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt) e executá-lo. Se o DefaultConnection for atualizado, podemos ver que as novas tabelas são adicionadas. Você pode verificar os dados dentro de tabelas para verificar se as informações foram migradas.

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image6.png)

## <a name="migrating-the-application-to-use-aspnet-identity"></a>Migrar o aplicativo para usar a identidade do ASP.NET

1. Instale os pacotes Nuget necessários para a identidade do ASP.NET:

    - Microsoft.AspNet.Identity.EntityFramework
    - Microsoft.AspNet.Identity.Owin
    - Microsoft.Owin.Host.SystemWeb
    - Microsoft.Owin.Security.Facebook
    - Microsoft.Owin.Security.Google
    - Microsoft.Owin.Security.MicrosoftAccount
    - Microsoft.Owin.Security.Twitter

   Para obter mais informações sobre como gerenciar pacotes Nuget podem ser encontradas [aqui](http://docs.nuget.org/docs/start-here/Managing-NuGet-Packages-Using-The-Dialog)
2. Para trabalhar com dados existentes na tabela, precisamos criar classes de modelo que mapeiam para as tabelas e conectá-las no sistema de identidade. Como parte do contrato de identidade, as classes de modelo devem implementam as interfaces definidas na dll Identity.Core ou podem estender a implementação existente dessas interfaces disponíveis no EntityFramework. Vamos usar as classes existentes para a função, os logons de usuário e declarações de usuário. Precisamos usar um usuário personalizado em nosso exemplo. Clique com botão direito no projeto e criar nova pasta 'IdentityModels'. Adicione uma nova classe de 'User', conforme mostrado abaixo:

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample5.cs)]

   Observe que o 'ProfileInfo' agora é uma propriedade na classe de usuário. Portanto, podemos usar a classe de usuário para trabalhar diretamente com os dados de perfil.

Copie os arquivos a **IdentityModels** e **IdentityAccount** pastas da fonte de download ( [ https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/UniversalProviders-Identity-Migrations ](https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/UniversalProviders-Identity-Migrations) ). Eles têm as classes de modelo restantes e as novas páginas necessárias para o usuário e gerenciamento de função usando as APIs de identidade do ASP.NET. A abordagem usada é semelhante à associação de SQL e a explicação detalhada pode ser encontrada [aqui](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md).

[!INCLUDE[](../../../includes/identity/alter-command-exception.md)]

## <a name="copying-profile-data-to-the-new-tables"></a>Copiando dados de perfil para as novas tabelas

Como mencionado anteriormente, é necessário desserializar os dados xml nas tabelas de perfis e armazená-los nas colunas da tabela AspNetUsers. As novas colunas foram criadas na tabela users na etapa anterior, portanto, tudo o que resta é preencher as colunas com os dados necessários. Para fazer isso, usaremos um aplicativo de console que é executado uma vez para preencher as colunas recém-criadas na tabela users.

1. Crie um novo aplicativo de console na solução existente.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image2.jpg)
2. Instale a versão mais recente do pacote do Entity Framework.
3. Adicione o aplicativo web criado acima como uma referência para o aplicativo de console. Para fazer isso clique com botão direito no projeto, 'Add References', em seguida, a solução, em seguida, clique no projeto e clicar em Okey.
4. Copie o código na classe Program.cs abaixo. Essa lógica lê os dados de perfil para cada usuário, serializa-lo como 'ProfileInfo' objeto e o armazena no banco de dados.

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample6.cs)]

   Alguns modelos usados são definidos na pasta 'IdentityModels' do projeto de aplicativo web, portanto, você deve incluir os namespaces correspondentes.
5. O código acima funciona no arquivo de banco de dados no aplicativo\_pasta de dados do projeto de aplicativo web criado nas etapas anteriores. Para fazer referência a ele, atualize a cadeia de caracteres de conexão no arquivo App. config do aplicativo de console com a cadeia de conexão no Web. config do aplicativo web. Também fornecem o caminho físico completo na propriedade 'AttachDbFilename'.
6. Abra um prompt de comando e navegue até a pasta bin do aplicativo de console acima. Execute o executável e examine a saída de log, conforme mostrado na imagem a seguir.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image3.jpg)
7. Abra a tabela 'AspNetUsers' no Gerenciador de servidores e verifique se os dados nas novas colunas que contêm as propriedades. Eles devem ser atualizados com os valores de propriedade correspondentes.

## <a name="verify-functionality"></a>Verificar a funcionalidade

Use as páginas de associação adicionados recentemente que são implementadas usando a identidade do ASP.NET para fazer logon de um usuário de banco de dados antigo. O usuário deve ser capaz de fazer logon usando as mesmas credenciais. Experimente outras funcionalidades, como adicionar OAuth, criando um novo usuário, alterar uma senha, adição de funções, adicione usuários a funções, etc.

Os dados de perfil para o usuário antigo e novos usuários devem ser recuperados e armazenados na tabela users. A tabela antiga não deve ser referenciada.

## <a name="conclusion"></a>Conclusão

O artigo descreveu o processo de migração de aplicativos web que usasse o modelo de provedor para associação de identidade do ASP.NET. Além disso, o artigo descritas migrando dados de perfil para os usuários a ser conectado o sistema de identidade. Deixe comentários abaixo para perguntas e problemas encontrados ao migrar seu aplicativo.

*Graças ao Rick Anderson e Robert McMurray revisão do artigo.*
