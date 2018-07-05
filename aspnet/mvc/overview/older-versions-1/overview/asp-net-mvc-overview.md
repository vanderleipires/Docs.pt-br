---
uid: mvc/overview/older-versions-1/overview/asp-net-mvc-overview
title: Visão geral do ASP.NET MVC | Microsoft Docs
author: microsoft
description: Saiba mais sobre as diferenças entre o aplicativo ASP.NET MVC e aplicativos de Web Forms do ASP.NET. Saiba como decidir quando criar um aplicativo ASP.NET MVC.
ms.author: aspnetcontent
ms.date: 01/27/2009
ms.assetid: 2dcb44a4-5cbf-4d62-b363-718104082d86
msc.legacyurl: /mvc/overview/older-versions-1/overview/asp-net-mvc-overview
msc.type: authoredcontent
ms.openlocfilehash: 6f40bd19e19f05b91fbee59aefb2914e623d6f25
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802325"
---
<a name="aspnet-mvc-overview"></a>Visão geral do ASP.NET MVC
====================
por [Microsoft](https://github.com/microsoft)

> Saiba mais sobre as diferenças entre o aplicativo ASP.NET MVC e aplicativos de Web Forms do ASP.NET. Saiba como decidir quando criar um aplicativo ASP.NET MVC.


O padrão de arquitetura do Model-View-Controller (MVC) separa um aplicativo em três componentes principais: o modelo, o modo de exibição e o controlador. A estrutura ASP.NET MVC fornece uma alternativa ao padrão de Web Forms do ASP.NET para a criação de aplicativos Web baseados no MVC. A estrutura ASP.NET MVC é uma estrutura de apresentação leve e altamente testável que (assim como acontece com aplicativos baseados em Web Forms) é integrado com os recursos existentes do ASP.NET, como páginas mestras e autenticação baseada em associação. A estrutura do MVC é definida na **System.Web.Mvc** namespace e é uma parte fundamental, com suporte das **System. Web** namespace.   
  
O MVC é um padrão de design padrão que muitos desenvolvedores estão familiarizados com. Alguns tipos de aplicativos Web serão beneficiados com a estrutura do MVC. Outros continuarão a usar o padrão de aplicativo ASP.NET tradicional baseado em Web Forms e postbacks. Outros tipos de aplicativos Web combina as duas abordagens; uma abordagem não exclui o outro.   
  
A estrutura MVC inclui os seguintes componentes:


[![Invocar uma ação do controlador que espera um valor de parâmetro](asp-net-mvc-overview/_static/image1.jpg)](asp-net-mvc-overview/_static/image1.png)

**Figura 01**: invocar uma ação do controlador que espera um valor de parâmetro ([clique para exibir a imagem em tamanho normal](asp-net-mvc-overview/_static/image2.png))


- **Modelos de**. Objetos de modelo são as partes do aplicativo que implementam a lógica do domínio de dados do aplicativo s. Muitas vezes, os objetos de modelo recuperam e armazenam o estado de modelo em um banco de dados. Por exemplo, um objeto de produto pode recuperar informações de um banco de dados, operar nele e, em seguida, gravar informações atualizadas de volta para uma tabela de produtos no SQL Server.

Em aplicativos pequenos, o modelo é geralmente uma separação conceitual em vez de física. Por exemplo, se o aplicativo apenas lê um conjunto de dados e o envia para o modo de exibição, o aplicativo não tem uma camada de modelo físico e classes associadas. Nesse caso, o conjunto de dados assume a função de um objeto de modelo.

- **Modos de exibição**. As exibições são os componentes que exibem a interface do usuário do aplicativo s (UI). Normalmente, essa interface do usuário é criado a partir de dados de modelo. Um exemplo seria uma exibição de edição de uma tabela de produtos que exibe as caixas de texto, listas suspensas e caixas de seleção com base no estado atual de um objeto de produtos.

- **Controladores**. Controladores são os componentes que manipular a interação do usuário, trabalham com o modelo e, finalmente, selecionam uma exibição para renderizar que exibe a interface do usuário. Em um aplicativo MVC, a exibição mostra apenas informações; o controlador manipula e responde à entrada e à interação do usuário. Por exemplo, o controlador manipula valores de cadeia de caracteres de consulta e passa esses valores para o modelo, que por sua vez, consulta o banco de dados usando os valores.

O padrão MVC ajuda você a criar aplicativos que separam os diferentes aspectos do aplicativo (lógica de entrada, lógica de negócios e lógica de interface do usuário), enquanto fornece um acoplamento flexível entre esses elementos. O padrão especifica onde cada tipo de lógica deve estar localizado no aplicativo. A lógica da interface do usuário pertence à exibição. A lógica de entrada pertence ao controlador. A lógica de negócios pertence ao modelo. Essa separação ajuda você a gerenciar a complexidade ao compilar um aplicativo, porque ela permite que você se concentre em um aspecto da implementação por vez. Por exemplo, você pode se concentrar na exibição sem depender da lógica de negócios.   
  
Além de gerenciar a complexidade, o padrão MVC torna mais fácil testar aplicativos do que testar um aplicativo Web baseado em Web Forms do ASP.NET. Por exemplo, em um aplicativo Web ASP.NET baseado em Web Forms, uma única classe é usada para exibir a saída e para responder à entrada do usuário. Escrever testes automatizados para aplicativos ASP.NET baseados em formulários da Web pode ser complexa, porque para testar uma página individual, você deve instanciar a classe de página, todos os seus controles filho e classes dependentes adicionais no aplicativo. Porque muitas classes sejam instanciadas para executar a página, pode ser difícil escrever testes que se concentrar exclusivamente em partes individuais do aplicativo. Testes para aplicativos ASP.NET baseados em Web Forms, portanto, podem ser mais difícil de implementar do que testes em um aplicativo MVC. Além disso, os testes em um aplicativo ASP.NET Web Forms-based exigem um servidor Web. A estrutura MVC separa os componentes e faz uso intenso de interfaces, o que torna possível testar componentes individuais isoladamente do resto do framework.   
  
O acoplamento flexível entre os três componentes principais de um aplicativo MVC também promove o desenvolvimento paralelo. Por exemplo, um desenvolvedor pode trabalhar no modo de exibição, um segundo desenvolvedor pode trabalhar na lógica do controlador e um terceiro desenvolvedor pode se concentrar na lógica de negócios no modelo.

## <a name="deciding-when-to-create-an-mvc-application"></a>Decidindo quando criar um aplicativo MVC

Você deve considerar cuidadosamente se deve implementar um aplicativo Web usando a estrutura ASP.NET MVC ou o modelo de Web Forms do ASP.NET. A estrutura do MVC não substitui o modelo de Web Forms; Você pode usar ambas as estruturas para aplicativos da Web. (Se você tiver aplicativos existentes baseados em Web Forms, eles continuarão a funcionar exatamente como de costume.)   
  
Antes de decidir usar a estrutura do MVC ou o modelo de Web Forms para um site específico, pondere as vantagens de cada abordagem.

### <a name="advantages-of-an-mvc-based-web-application"></a>Vantagens de um aplicativo Web com base no MVC

O ASP.NET MVC framework oferece as seguintes vantagens:

- Ele torna mais fácil de gerenciar a complexidade ao dividir um aplicativo em modelo, o modo de exibição e o controlador.
- Ele não usa estado de exibição nem formulários baseados em servidor. Isso torna a estrutura MVC ideal para desenvolvedores que desejam controle completo sobre o comportamento de um aplicativo.
- Ele usa um padrão Front Controller que processa as solicitações de aplicativo Web por meio de um único controlador. Isso permite que você criar um aplicativo que dá suporte a uma infraestrutura de roteamento avançada. Para obter mais informações, consulte [controlador frontal](https://go.microsoft.com/fwlink/?LinkId=106357 "controlador frontal") no site do MSDN.
- Ele fornece melhor suporte para o desenvolvimento orientado por testes (TDD).
- Ele funciona bem para aplicativos da Web que são suportados por grandes equipes de desenvolvedores e designers da Web que precisam de um alto grau de controle sobre o comportamento do aplicativo.

### <a name="advantages-of-a-web-forms-based-web-application"></a>Vantagens de um aplicativo da Web baseada em formulários da Web

A estrutura baseada em Web Forms oferece as seguintes vantagens:

- Ele dá suporte a um modelo de evento que preserva o estado sobre HTTP, que beneficia o desenvolvimento de aplicativos Web de linha de negócios. O aplicativo baseado em Web Forms fornece dezenas de eventos que são suportados em centenas de controles de servidor.
- Ele usa um padrão Page Controller que adiciona funcionalidade para páginas individuais. Para obter mais informações, consulte [Page Controller](https://go.microsoft.com/fwlink/?LinkId=106359 "Page Controller") no site do MSDN.
- Ele usa o estado de exibição nem formulários baseados em servidor, que podem facilitar o gerenciamento de informações de estado.
- Ele funciona bem para pequenas equipes de desenvolvedores da Web e designers que desejam tirar proveito do grande número de componentes disponíveis para desenvolvimento rápido de aplicativos.
- Em geral, é menos complexo para o desenvolvimento de aplicativos, porque os componentes (a **página** classe, controles e assim por diante) estão extremamente integrados e normalmente exigem menos código do que o modelo MVC.

## <a name="features-of-the-aspnet-mvc-framework"></a>Recursos da estrutura do ASP.NET MVC

A estrutura ASP.NET MVC fornece os seguintes recursos:

- Separação de tarefas do aplicativo (lógica de entrada, lógica de negócios e lógica de interface do usuário), possibilidade de teste e desenvolvimento orientado por testes (TDD) por padrão. Todos os contratos núcleo na estrutura MVC são baseados em interface e podem ser testados usando objetos fictícios, que são objetos simulados que imitam o comportamento dos objetos reais no aplicativo. Você pode de teste de unidade do aplicativo sem a necessidade de executar os controladores em um processo do ASP.NET, o que torna rápido e flexível de teste de unidade. Você pode usar qualquer estrutura de teste de unidade que seja compatível com o .NET Framework.
- Uma estrutura extensível e conectável. Os componentes da estrutura MVC do ASP.NET são projetados para que eles podem ser facilmente substituídos ou personalizados. Você pode conectar seu próprio mecanismo de exibição, política de roteamento de URL, serialização de parâmetro do método de ação e outros componentes. A estrutura ASP.NET MVC também suporta o uso de modelos de contêiner de injeção de dependência (DI) e inversão de controle (IOC). Injeção de dependência permite injetar objetos em uma classe, em vez de depender da classe para criar o próprio objeto. A IOC Especifica que, se um objeto requer outro objeto, os primeiros objetos devem obter o segundo objeto de uma fonte externa, como um arquivo de configuração. Isso facilita os testes.
- Um componente de mapeamento de URL poderoso que permite que você crie aplicativos que têm URLs compreensíveis e pesquisáveis. URLs não precisa incluir extensões de nome de arquivo e são projetadas para dar suporte a padrões de nomenclatura de URL que funcionam bem para search engine optimization (SEO) e endereçamento de transferência de estado representacional (REST).
- Suporte para usar a marcação na página existente do ASP.NET (arquivos. aspx), o controle de usuário (arquivos. ascx) e arquivos de marcação de (arquivos. master) página mestra como modelos de exibição. Você pode usar recursos ASP.NET existentes com a estrutura do ASP.NET MVC, como páginas mestras aninhadas, expressões em linha (&lt;% = %&gt;), controles de servidor declarativos, modelos, vinculação de dados, localização e assim por diante.
- Suporte para recursos ASP.NET existentes. ASP.NET MVC permite que você use recursos como autenticação de formulários e autenticação do Windows, a autorização de URL, associação e funções, saída e cache de dados, gerenciamento de estado de sessão e perfil, monitoramento de integridade, o sistema de configuração e o provedor arquitetura.
