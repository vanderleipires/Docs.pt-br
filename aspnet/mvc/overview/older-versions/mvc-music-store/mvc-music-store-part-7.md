---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
title: "Parte 7: Associação e a autorização | Microsoft Docs"
author: jongalloway
description: "Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo do repositório de música do ASP.NET MVC. Parte 7 abrange a associação e a autorização."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/13/2010
ms.topic: article
ms.assetid: c8511ebe-68bc-4240-87c3-d5ced84a3f37
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
msc.type: authoredcontent
ms.openlocfilehash: db459de687db862be00a9b59ff5b1b238fa75061
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
<a name="part-7-membership-and-authorization"></a>Parte 7: Associação e a autorização
====================
por [Jon Galloway](https://github.com/jongalloway)

> O repositório de música MVC é um aplicativo tutorial que apresenta e explica passo a passo sobre como usar o ASP.NET MVC e o Visual Studio para desenvolvimento na web.  
>   
> O repositório de música MVC é uma implementação de repositório de exemplo leve que vende álbuns de música online e implementa a administração de site básico, entrada do usuário e funcionalidade do carrinho de compras.  
>   
> Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo do repositório de música do ASP.NET MVC. Parte 7 abrange a associação e a autorização.


Nosso controlador do Gerenciador de armazenamento é acessível para qualquer pessoa que visite nosso site no momento. Vamos alterar isso para restringir a permissão para administradores de site.

## <a name="adding-the-accountcontroller-and-views"></a>Adicionando o AccountController e modos de exibição

Uma diferença entre o modelo de aplicativo Web do ASP.NET MVC 3 completo e o modelo de aplicativo de Web vazio do ASP.NET MVC 3 é que o modelo vazio não inclui um controlador de conta. Vamos adicionar um controlador de conta, copie alguns arquivos de um novo aplicativo ASP.NET MVC criado usando o modelo de aplicativo Web do ASP.NET MVC 3 completo.

Criar um novo aplicativo ASP.NET MVC usando o modelo de aplicativo Web do ASP.NET MVC 3 completo e copie os seguintes arquivos para os mesmos diretórios em nosso projeto:

1. Copiar AccountController.cs no diretório controladores
2. Copiar AccountModels no diretório de modelos
3. Crie um diretório de conta dentro do diretório de modos de exibição e copie todos os quatro modos de exibição no

Altere o namespace para as classes do controlador e o modelo de modo que começam com MvcMusicStore. A classe AccountController deve usar o namespace MvcMusicStore.Controllers, e a classe AccountModels deve usar o namespace MvcMusicStore.Models.

*Observação: Esses arquivos também estão disponíveis no download do MvcMusicStore Assets.zip do qual é copiado os arquivos de design do site no início do tutorial. Os arquivos de associação estão localizados no diretório de código.*

A solução atualizada deve parecer com o seguinte:

![](mvc-music-store-part-7/_static/image1.png)

## <a name="adding-an-administrative-user-with-the-aspnet-configuration-site"></a>Adicionando um usuário administrativo com o site de configuração do ASP.NET

Antes que exigem autorização em nosso site, precisaremos criar um usuário com acesso. A maneira mais fácil de criar um usuário é usar o site de configuração do ASP.NET interno.

Inicie o site de configuração do ASP.NET, clicando no ícone no Gerenciador de soluções a seguir.

![](mvc-music-store-part-7/_static/image2.png)

Isso inicia um site de configuração. Clique na guia Segurança, na tela inicial, clique no link "Habilitar funções" no centro da tela.

![](mvc-music-store-part-7/_static/image3.png)

Clique no link "criar ou gerenciar funções".

![](mvc-music-store-part-7/_static/image4.png)

Insira "Administrador" como o nome da função e pressione o botão Adicionar função.

![](mvc-music-store-part-7/_static/image5.png)

Clique no botão Voltar, em seguida, clique no link Criar usuário no lado esquerdo.

![](mvc-music-store-part-7/_static/image6.png)

Preencha os campos de informações do usuário à esquerda usando as seguintes informações:

| **Campo** | **Value** |
| --- | --- |
| **Nome de usuário** | Administrador |
| **Senha** | password123! |
| **Confirmar senha** | password123! |
| **Email** | (qualquer endereço de email funcionarão) |
| **Pergunta de Segurança** | (o que quiser) |
| **Resposta de Segurança** | (o que quiser) |

*Observação: Você pode usar claro qualquer senha que você deseja. As configurações de segurança de senha padrão exigem uma senha que 7 caracteres e contém um caractere não alfanumérico.*

Selecione a função de administrador para este usuário e clique no botão Criar usuário.

![](mvc-music-store-part-7/_static/image7.png)

Neste ponto, você verá uma mensagem indicando que o usuário foi criado com êxito.

![](mvc-music-store-part-7/_static/image8.png)

Agora você pode fechar a janela do navegador.

## <a name="role-based-authorization"></a>Autorização baseada em função

Agora podemos pode restringir o acesso para o StoreManagerController usando o atributo [autorizar], especificando que o usuário deve ser da função de administrador para acessar qualquer ação do controlador na classe.

[!code-csharp[Main](mvc-music-store-part-7/samples/sample1.cs)]

*Observação: O atributo [autorizar] pode ser colocado em métodos de ação específica, bem como no nível de classe do controlador.*

Procurando /StoreManager agora traz uma caixa de diálogo de logon:

![](mvc-music-store-part-7/_static/image9.png)

Depois de fazer logon com a nova conta de administrador, podemos ir para a tela Editar álbum como antes.

>[!div class="step-by-step"]
[Anterior](mvc-music-store-part-6.md)
[Próximo](mvc-music-store-part-8.md)
