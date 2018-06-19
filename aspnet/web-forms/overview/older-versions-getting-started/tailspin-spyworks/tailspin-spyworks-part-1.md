---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: 'Parte 1: Arquivo -> Novo projeto | Microsoft Docs'
author: JoeStagner
description: Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo Tailspin Spyworks. Parte 1 abrange visão geral e o arquivo/novo projeto.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: a1b9681516e626b6a0eec420b168a74e05d88afb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892459"
---
<a name="part-1-file--new-project"></a>Parte 1: Arquivo -> Novo projeto
====================
por [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks demonstra como extraordinariamente simples é criar aplicativos eficientes e escalonáveis para a plataforma .NET. Ele mostra como usar os novos recursos no ASP.NET 4 para criar uma loja online, inclusive de compras, check-out e administração.
> 
> Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo Tailspin Spyworks. Parte 1 abrange visão geral e o arquivo/novo projeto.


## <a id="_Toc260221666"></a>  Visão geral

Este tutorial é uma introdução aos formulários da Web do ASP.NET. Nós começarão lenta, para que a experiência de desenvolvimento de web de nível para iniciantes é okey.

O aplicativo que é criarei é um repositório online simple.

![](tailspin-spyworks-part-1/_static/image1.jpg)


Os visitantes podem procurar produtos por categoria:

![](tailspin-spyworks-part-1/_static/image2.jpg)

Eles podem exibir um único produto e adicioná-lo ao seu carrinho:

![](tailspin-spyworks-part-1/_static/image3.jpg)

Eles podem revisar seu carrinho, removendo todos os itens que não desejam mais:

![](tailspin-spyworks-part-1/_static/image4.jpg)

Continuar o check-out Avisar

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

Após a ordenação, eles veem uma tela de confirmação simples:

![](tailspin-spyworks-part-1/_static/image7.jpg)


Vamos começar criando um novo projeto de formulários da Web do ASP.NET no Visual Studio 2010 e adicionaremos incrementalmente recursos para criar um aplicativo funcional completo. Ao longo do caminho, vamos abordar o acesso de banco de dados, modos de exibição de lista e grade, páginas de atualização de dados, validação de dados, usando páginas mestras para layout de página consistente, AJAX, validação, a associação do usuário e muito mais.

Você pode acompanhá-lo passo a passo, ou você pode baixar o aplicativo concluído [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)

Você pode usar o Visual Studio 2010 ou o livre Visual Web Developer 2010 de [ https://www.microsoft.com/express/Web/ ](https://www.microsoft.com/express/Web/). Para criar o aplicativo, você pode usar o SQL Server ou o livre SQL Server Express para hospedar o banco de dados.

## <a id="_Toc260221667"></a>  Arquivo / novo projeto

Vamos começar selecionando o novo projeto no menu arquivo no Visual Studio. Isso abre a caixa de diálogo Novo projeto.

![](tailspin-spyworks-part-1/_static/image8.jpg)

Selecionaremos Visual C# modelos da Web de grupo à esquerda e, em seguida, escolha o modelo de "Aplicativo Web do ASP.NET" na coluna central. Nomeie o projeto TailspinSpyworks e pressione o botão Okey.

![](tailspin-spyworks-part-1/_static/image9.jpg)

Isso criará o nosso projeto. Vamos analisar as pastas que são incluídas em nosso aplicativo no Gerenciador de soluções, no lado direito.

![](tailspin-spyworks-part-1/_static/image10.jpg)

A solução vazia não é totalmente vazia – adiciona uma estrutura de pasta básica:

![](tailspin-spyworks-part-1/_static/image1.png)

Observe as convenções implementadas pelo modelo de projeto padrão do ASP.NET 4.

- A pasta "Conta" implementa uma interface do usuário básica para ASP. Subsistema de associação do NET.
- A pasta "Scripts" serve como o repositório de arquivos de JavaScript do lado cliente e os principais jQuery. js arquivos estão disponíveis por padrão.
- A pasta "Estilos" é usada para organizar nossos visuais de site da web (folhas de estilo CSS)

Quando, pressione F5 para executar o nosso aplicativo e renderizar a página default.aspx vemos a seguir.

![](tailspin-spyworks-part-1/_static/image11.jpg)

Nosso primeira aprimoramento de aplicativo será para substituir o arquivo Style.css do modelo de formulários da Web padrão com os arquivos de imagem associado que processarão o asthetics visual que queremos para nosso aplicativo Tailspin Spyworks e classes CSS.

Depois de fazer isso nossa página de default.aspx renderiza assim.

![](tailspin-spyworks-part-1/_static/image12.jpg)

Observe os links de imagem na parte superior direita da página e os itens de menu que foram adicionados para a página mestra. Somente os links de "Entrar" e "Conta" apontam para páginas que existem (geradas pelo modelo padrão) e o restante das páginas que vamos implementar durante a compilação de nosso aplicativo.

Também vamos realocar a página mestra para o diretório de estilos. Embora essa seja uma preferência somente pode fazer coisas um pouco mais fácil se decida fazer nosso aplicativo "skinable" no futuro.

Depois de fazer isso, precisamos alterar a página mestra referências em todos os arquivos. aspx gerado pelo padrão páginas ASP.NET WebForms.

> [!div class="step-by-step"]
> [Avançar](tailspin-spyworks-part-2.md)
