---
uid: web-forms/what-is-web-forms
title: "O que é o Web Forms | Microsoft Docs"
author: rick-anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/21/2014
ms.topic: article
ms.assetid: 5fa1daf9-1161-4cfa-bd4c-658f48b2c229
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/what-is-web-forms
msc.type: content
ms.openlocfilehash: dcaba60a8640ce83f73460a5c8ee457257b9397e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="what-is-web-forms"></a>O que é o Web Forms
====================
Web Forms do ASP.NET é uma parte da estrutura de aplicativo web do ASP.NET e está incluído com [Visual Studio](https://www.asp.net/downloads). É um de quatro modelos de programação, que você pode usar para criar aplicativos web ASP.NET, os outros são ASP.NET MVC, páginas da Web ASP.NET e aplicativos de página única do ASP.NET.

Formulários da Web são páginas que os usuários solicitam usando seu navegador. Essas páginas podem ser escritas usando uma combinação de HTML, script de cliente, controles de servidor e código do servidor. Quando os usuários solicitam uma página, ele é compilado e executado no servidor pela estrutura e, em seguida, a estrutura gera a marcação HTML que pode fazer com que o navegador. Uma página de Web Forms do ASP.NET apresenta informações para o usuário em qualquer navegador ou dispositivo cliente.

Usando o Visual Studio, você pode criar Web Forms do ASP.NET. O Visual Studio Integrated Development Environment (IDE) permite que você arrastar e soltar os controles de servidor para definir o layout de sua página de Web Forms. Você pode facilmente definir propriedades, métodos e eventos para controles na página ou para o próprio de página. Essas propriedades, métodos e eventos são usados para definir o comportamento da página da web, aparência e assim por diante. Para escrever código para manipular a lógica para a página do servidor, você pode usar uma linguagem .NET, como Visual Basic ou c#.

> [!NOTE] 
> 
> Documentação do ASP.NET e Visual Studio abrange várias versões. Os tópicos que realçam os recursos de versões anteriores podem ser úteis para seus cenários usando as versões mais recentes e as tarefas atuais.


**Web Forms do ASP.NET são:** 

- Com base na tecnologia do Microsoft ASP.NET, no qual o código que é executado no servidor dinamicamente gera saída de página da Web para o dispositivo cliente ou navegador.
- Compatível com qualquer navegador ou dispositivo móvel. Uma página da Web ASP.NET automaticamente processa o HTML compatível com o navegador correto para recursos como estilos, layout e assim por diante.
- Compatível com qualquer linguagem com suporte pelo .NET common language runtime, como Microsoft Visual Basic e Microsoft Visual c#.
- Criado no Microsoft .NET Framework. Isso fornece todos os benefícios do framework, incluindo um ambiente gerenciado, a segurança de tipo e a herança.
- Flexível porque você pode adicionar criados pelo usuário e de terceiros controla a eles.

**Oferta de Web Forms do ASP.NET:** 

- Separação de HTML e outro código de interface do usuário da lógica do aplicativo.
- Um conjunto rico de controles de servidor para tarefas comuns, incluindo acesso a dados.
- Associação com suporte excelente ferramenta de dados avançados.
- Suporte para scripts do lado do cliente que é executado no navegador.
- Suporte para uma variedade de outros recursos, incluindo roteamento, segurança, desempenho, internacionalização, testar, depurar, tratamento de erros e gerenciamento de estado.

## <a name="aspnet-web-forms-helps-you-overcome-challenges"></a>Web Forms do ASP.NET ajuda você a superar os desafios

Programação de aplicativo Web apresenta desafios que normalmente não surgem ao programar aplicativos tradicionais baseados no cliente. Entre os desafios são:

- **Implementando uma interface de usuário da Web avançada** - pode ser difícil e tedioso para criar e implementar um usuário usando recursos HTML básicos, especialmente se a página tem um layout complexo, uma grande quantidade de conteúdo dinâmico, de interface e completo objetos de usuário interativo.
- **Separação entre cliente e servidor** -aplicativo na Web, o cliente (navegador) e o servidor são programas diferentes geralmente executados em computadores diferentes (e até mesmo em diferentes sistemas operacionais). Consequentemente, as duas metades do aplicativo compartilhem muito poucas informações; eles podem se comunicar, mas normalmente trocam apenas pequenos blocos de informações simples.
- **Execução sem monitoração de estado** - quando um servidor Web recebe uma solicitação para uma página, ele encontra a página, processa-os, envia para o navegador e, em seguida, descarta todas as informações de página. Se o usuário solicita a mesma página novamente, o servidor repete a sequência inteira, reprocessamento a página a partir do zero. Ou seja, um servidor não tem memória das páginas que ele tenha processado — página são sem monitoração de estado. Portanto, se um aplicativo precisar manter informações sobre uma página, sua natureza sem monitoração de estado pode se tornar um problema.
- **Recursos de cliente desconhecido** -em muitos casos, os aplicativos da Web são acessíveis a vários usuários usando diferentes navegadores. Navegadores têm recursos diferentes, tornando difícil criar um aplicativo que será executado igualmente bem em todos eles.
- **Complicações com acesso a dados** -ler e gravar em uma fonte de dados em aplicativos Web tradicionais podem ser complicadas e de uso intensivo de recursos.
- **Complicações com escalabilidade** – em muitos casos, os aplicativos Web criados com os métodos existentes não atendem aos objetivos de escalabilidade devido à falta de compatibilidade entre os vários componentes do aplicativo. Isso geralmente é um ponto de falha comuns para aplicativos em um ciclo pesado de crescimento.

Enfrentar esses desafios para aplicativos da Web pode exigir significativa de tempo e esforço. Web Forms do ASP.NET e a estrutura do ASP.NET enfrentam esses desafios das seguintes maneiras:

- **Modelo de objeto intuitivo e consistente** -estrutura de página no ASP.NET apresenta um modelo de objeto que permite que você pense seus formulários como uma unidade, não como partes separadas de cliente e servidor. Nesse modelo, você pode programar a página de forma mais intuitiva que em aplicativos Web tradicionais, incluindo a capacidade de definir as propriedades de elementos da página e responder a eventos. Além disso, os controles de servidor ASP.NET são uma abstração do conteúdo físico de uma página HTML e da interação direta entre o navegador e o servidor. Em geral, você pode usar controles de servidor a maneira como você pode trabalhar com controles em um aplicativo cliente e não precisará pensar sobre como criar o HTML para apresentar e processar os controles e seu conteúdo.
- **Modelo de programação controlada por evento** -trazer de Web Forms do ASP.NET para aplicativos Web o modelo familiar de gravar manipuladores de eventos para eventos que ocorrem no cliente ou servidor. A estrutura de página ASP.NET abstrai esse modelo de forma que o mecanismo subjacente de capturar um evento no cliente, transmiti-los para o servidor e chamar o método apropriado é automático e invisível para você. O resultado é uma estrutura código clara e facilmente escrita que dá suporte ao desenvolvimento controlado por evento.
- **Gerenciamento de estado intuitivo** - estrutura de página no ASP.NET manipula automaticamente a tarefa de manter o estado da sua página e seus controles e ele lhe fornece maneiras explícita para manter o estado de informações específicas do aplicativo. Isso é feito sem uso intenso de recursos do servidor e pode ser implementado com ou sem enviar cookies no navegador.
- **Aplicativos independentes de navegador** -estrutura de página do ASP.NET permite que você crie toda a lógica de aplicativo no servidor, eliminando a necessidade de codificação explícita para descobrir diferenças nos navegadores. No entanto, ele ainda permite aproveitar os recursos específicos de navegador escrevendo código do lado do cliente para fornecer melhor desempenho e uma experiência mais rica do cliente.
- **Suporte de tempo de execução de linguagem comum do .NET framework** -estrutura de página no ASP.NET é criado no .NET Framework, de forma a estrutura inteira está disponível para qualquer aplicativo ASP.NET. Os aplicativos podem ser gravados em qualquer linguagem que é compatível com o tempo de execução. Além disso, o acesso a dados é simplificado usando a infraestrutura de acesso de dados fornecida pelo .NET Framework, inclusive o ADO.NET.
- **Desempenho do servidor escalonável do .NET framework** -estrutura de página do ASP.NET permite que você dimensione seu aplicativo da Web de um computador com um único processador para um Web farm com vários computador corretamente e sem alterações complicadas no aplicativo lógica.

## <a name="features-of-aspnet-web-forms"></a>Recursos do Web Forms do ASP.NET

- **Controles de servidor**-controles de servidor Web do ASP.NET são objetos nas páginas da Web ASP.NET que são executados quando a página é solicitada e a marcação que processamento para o navegador. Vários controles de servidor Web são semelhantes aos elementos do HTML, como botões e caixas de texto. Outros controles possuem comportamento complexo, como um calendário de controles e que você pode usar para se conectar a fontes de dados e exibir dados.
- **Páginas mestras**-páginas mestras ASP.NET permitem que você crie um layout consistente para as páginas em seu aplicativo. Uma única página mestra define a aparência e o comportamento padrão que você deseja para todas as páginas (ou um grupo de páginas) em seu aplicativo. Em seguida, você pode criar páginas de conteúdo individuais que contêm o conteúdo que você deseja exibir. Quando os usuários solicitam as páginas de conteúdo, eles mesclam com a página mestra para produzir saída que combina o layout da página mestra com o conteúdo da página de conteúdo.
- **Trabalhando com dados**-ASP.NET fornece muitas opções para armazenar, recuperar e exibir dados. Em um aplicativo de Web Forms do ASP.NET, você pode usar controles de associação de dados para automatizar a entrada de dados em elementos de interface do usuário da página da web, como tabelas e caixas de texto e listas suspensas ou apresentação.
- **Associação**-identidade ASP.NET armazena as credenciais de usuários em um banco de dados criado pelo aplicativo. Quando os usuários fazem logon, o aplicativo valida suas credenciais ao ler o banco de dados. Seu projeto *conta* pasta contém os arquivos que implementam as diversas partes da associação: registrar, logon, alterar uma senha e autorizar o acesso. Além disso, o ASP.NET Web Forms oferece suporte a OpenID e OAuth. Esses aprimoramentos de autenticação permitem que os usuários entrem no seu site usando as credenciais existentes, de tal contas como Facebook, Twitter, Windows Live e Google. Por padrão, o modelo cria um banco de dados de associação usando um nome de banco de dados padrão em uma instância do SQL Server Express LocalDB, o servidor de banco de dados de desenvolvimento que acompanha o Visual Studio Express 2013 para Web.
- **Script de cliente e estruturas de cliente**-você pode aprimorar os recursos baseados em servidor do ASP.NET, incluindo a funcionalidade de script de cliente em páginas ASP.NET Web Form. Você pode usar o script de cliente para fornecer uma interface de usuário mais rica e mais ágil na resposta para os usuários. Você também pode usar o script de cliente para fazer chamadas assíncronas para o servidor Web enquanto uma página está em execução no navegador.
- **Roteamento**-roteamento de URL permite que você configure um aplicativo para aceitar solicitações de URLs que não mapeiam para arquivos físicos. Uma URL de solicitação é simplesmente a URL de um usuário entra no seu navegador para localizar uma página no site da web. Use o roteamento para definir URLs que são semanticamente significativo para usuários e que podem ajudar com o mecanismo de pesquisa SEO (otimização).
- **Gerenciamento de estado**-Web Forms do ASP.NET inclui várias opções para ajudarão-lo a preservam dados em uma base por página e uma base de todo o aplicativo.
- **Segurança**-uma parte importante do desenvolvimento de um aplicativo mais seguro é compreender as ameaças a ele. A Microsoft desenvolveu uma maneira para categorizar as ameaças: falsificação, violação, repúdio, divulgação de informações, negação de serviço, elevação de privilégio (STRIDE). No Web Forms do ASP.NET, você pode adicionar pontos de extensibilidade e opções de configuração que permitem personalizar vários comportamentos de segurança em Web Forms do ASP.NET.
- **Desempenho**-desempenho pode ser um fator chave em um site bem-sucedido ou projeto. Web Forms do ASP.NET permite que você modifique o desempenho relacionado à página e processamento do servidor de controle, gerenciamento de estado, acesso a dados, configuração de aplicativo e carregamento e eficiente de práticas recomendadas de codificação.
- **Internacionalização**- Web Forms do ASP.NET permite que você crie páginas da web que pode obter conteúdo e outros dados com base na configuração de idioma preferencial para o navegador ou em escolha explícita de idioma do usuário. Conteúdo e outros dados são referidos como recursos e esses dados podem ser armazenadas em arquivos de recurso ou outras fontes. Em uma página de Web Forms do ASP.NET, você deve configurar os controles para obter seus valores de propriedade dos recursos. Em tempo de execução, as expressões de recurso são substituídas por recursos do arquivo de recurso localizado apropriado.
- **Depuração e tratamento de erros**-ASP.NET inclui recursos para ajudá-lo a diagnosticar problemas que podem surgir em seu aplicativo Web Forms. Depuração e tratamento de erros também têm suporte em Web Forms do ASP.NET para que seus aplicativos compilar e executar com eficiência.
- **Implantação e hospedagem**-Visual Studio, o ASP.NET, o Azure e o IIS fornecem ferramentas que ajudam com o processo de implantação e hospedando o seu aplicativo de Web Forms.

## <a name="deciding-when-to-create-a-web-forms-application"></a>Decidindo quando criar um aplicativo Web Forms

Você deve considerar cuidadosamente se implementar um aplicativo Web usando o ASP.NET Web Forms modelo ou outro, como a estrutura ASP.NET MVC. A estrutura MVC não substitui o modelo de Web Forms. Você pode usar qualquer estrutura para aplicativos Web. Antes de você decidir usar o modelo de formulários da Web ou a estrutura do MVC para um site específico, pondere as vantagens de cada abordagem.

### <a name="advantages-of-a-web-forms-based-web-application"></a>Vantagens de um aplicativo da Web com base em formulários da Web

A estrutura baseada em Web Forms oferece as seguintes vantagens:

- Ele dá suporte a um modelo de evento que preserva o estado sobre HTTP, que beneficia o desenvolvimento de aplicativos Web de linha de negócios. O aplicativo baseado em Web Forms fornece dezenas de eventos que são suportados em centenas de controles de servidor.
- Ele usa um padrão Page Controller que adiciona funcionalidade em páginas individuais. Para obter mais informações, consulte [Page Controller](https://go.microsoft.com/fwlink/?LinkId=106359 "Page Controller") no site do MSDN.
- Ele usa o estado de exibição nem formulários baseados no servidor, o que podem tornar mais fácil gerenciar informações de estado.
- Ela funciona bem para pequenas equipes de desenvolvedores e Web designers que desejam tirar proveito do grande número de componentes disponíveis para o desenvolvimento rápido de aplicativos.
- Em geral, é menos complexo para o desenvolvimento de aplicativos, porque os componentes (a **página** classe, controles e assim por diante) são integrados e geralmente exigem menos código do que o modelo MVC.

### <a name="advantages-of-an-mvc-based-web-application"></a>Vantagens de um aplicativo Web baseado em MVC

A estrutura ASP.NET MVC oferece as seguintes vantagens:

- Ele torna mais fácil gerenciar a complexidade ao dividir um aplicativo em modelo, o modo de exibição e o controlador.
- Não usa o estado de exibição nem formulários baseados no servidor. Isso torna a estrutura MVC ideal para desenvolvedores que desejam controle completo sobre o comportamento de um aplicativo.
- Ele usa um padrão Front Controller que processa solicitações de aplicativo da Web por meio de um único controlador. Isso permite que você criar um aplicativo que oferece suporte a uma poderosa infraestrutura de roteamento. Para obter mais informações, consulte [Front Controller](https://go.microsoft.com/fwlink/?LinkId=106357 "Front Controller") no site do MSDN.
- Ele fornece um melhor suporte para desenvolvimento controlado por testes (TDD).
- Ela funciona bem para aplicativos da Web que são suportados por grandes equipes de desenvolvedores e Web designers que precisam de um alto grau de controle sobre o comportamento do aplicativo.
