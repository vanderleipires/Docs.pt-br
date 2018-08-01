---
uid: mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-cs
title: 'Iteração #4 – tornar o aplicativo fracamente acoplado (c#) | Microsoft Docs'
author: microsoft
description: Nesta quarta iteração, podemos tirar proveito dos diversos padrões de design de software para facilitar a manutenção e modificar o aplicativo Gerenciador de contatos. Para...
ms.author: aspnetcontent
ms.date: 02/20/2009
ms.assetid: 829f589f-e201-4f6e-9ae6-08ae84322065
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-cs
msc.type: authoredcontent
ms.openlocfilehash: 7c06609afd6f1adf930a377c99d66937885f78e7
ms.sourcegitcommit: a25b572eaed21791230c85416f449f66a405ec19
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39396018"
---
<a name="iteration-4--make-the-application-loosely-coupled-c"></a>Iteração #4 – tornar o aplicativo fracamente acoplado (c#)
====================
por [Microsoft](https://github.com/microsoft)

[Baixar o código](iteration-4-make-the-application-loosely-coupled-cs/_static/contactmanager_4_cs1.zip)

> Nesta quarta iteração, podemos tirar proveito dos diversos padrões de design de software para facilitar a manutenção e modificar o aplicativo Gerenciador de contatos. Por exemplo, podemos refatorar nosso aplicativo para usar o padrão de repositório e o padrão de injeção de dependência.


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Criando um aplicativo ASP.NET MVC de gerenciamento de contatos (c#)

Esta série de tutoriais, vamos criar um aplicativo de gerenciamento de contatos inteiro do início ao fim. O aplicativo Gerenciador de contatos permite que você armazene informações de contato - nomes, números de telefone e endereços de email - para obter uma lista de pessoas.

Criamos o aplicativo ao longo de várias iterações. Com cada iteração, podemos melhorar gradualmente o aplicativo. A meta dessa abordagem de iteração vários é que você possa entender o motivo para cada alteração.

- Iteração #1 - criar o aplicativo. A primeira iteração, podemos criar o Gerenciador de contatos da maneira mais simples possível. Adicionamos suporte para operações de banco de dados básico: criar, ler, atualizar e excluir (CRUD).

- Iteração #2 - tornar o aplicativo interessante. Nesta iteração, podemos melhorar a aparência do aplicativo modificando a página mestra do ASP.NET MVC exibição padrão e em cascata de folha de estilos.

- Iteração #3 - adicionar validação de formulário. Na terceira iteração, podemos adicionar validação de formulário básico. Podemos impedir que pessoas enviando um formulário sem preencher os campos obrigatórios do formulário. Podemos também validar endereços de email e números de telefone.

- Iteração #4 – tornar o aplicativo fracamente acoplado. Nesta quarta iteração, podemos tirar proveito dos diversos padrões de design de software para facilitar a manutenção e modificar o aplicativo Gerenciador de contatos. Por exemplo, podemos refatorar nosso aplicativo para usar o padrão de repositório e o padrão de injeção de dependência.

- Iteração #5 - criar testes de unidade. Na quinta iteração, podemos tornar nosso aplicativo mais fácil de manter e modificar adicionando testes de unidade. Vamos simular a nossas classes de modelo de dados e criar testes de unidade para nossos controladores e lógica de validação.

- Iteração #6 – usar desenvolvimento controlado por teste. Essa iteração sexta, adicionamos novas funcionalidades ao nosso aplicativo escrevendo testes de unidade pela primeira vez e escrever código contra os testes de unidade. Essa iteração, adicionamos os grupos de contatos.

- Iteração #7 - adicionar a funcionalidade do Ajax. A sétima iteração, podemos melhorar a capacidade de resposta e o desempenho do nosso aplicativo, adicionando suporte para Ajax.

## <a name="this-iteration"></a>Essa iteração

Nesta quarta iteração do aplicativo Contact Manager, podemos refatorar o aplicativo para tornar o aplicativo mais fracamente acoplado. Quando um aplicativo está acoplado livremente, você pode modificar o código em uma parte do aplicativo sem a necessidade de modificar o código em outras partes do aplicativo. Aplicativos flexíveis são mais resistentes a alterar.

Atualmente, toda a lógica de acesso e a validação de dados usada pelo aplicativo Contact Manager está contida nas classes do controlador. Isso é uma má ideia. Sempre que você precisa modificar uma parte do seu aplicativo, você corre o risco de introduzir bugs em outra parte do seu aplicativo. Por exemplo, se você modificar sua lógica de validação, você corre o risco introduzir novos bugs em sua lógica de acesso ou controlador de dados.

> [!NOTE] 
> 
> (SRP), uma classe nunca deve ter mais de um motivo para alterar. Mistura de controlador, validação e lógica de banco de dados é uma violação maciça do princípio da responsabilidade única.


Há várias razões pelas quais você talvez precise modificar seu aplicativo. Você talvez precise adicionar um novo recurso ao seu aplicativo, talvez você precise corrigir um bug em seu aplicativo ou você talvez precise modificar como um recurso do seu aplicativo é implementado. Os aplicativos são raramente estáticos. Eles tendem a crescer e seja modificado ao longo do tempo.

Por exemplo, imagine que você decidir alterar como você implementa sua camada de acesso a dados. Direita agora, o aplicativo Contact Manager usa o Entity Framework da Microsoft para acessar o banco de dados. No entanto, você pode decidir migrar para uma tecnologia de acesso de dados novas ou alternativas, como serviços de dados ADO.NET ou NHibernate. No entanto, como o código de acesso a dados não é isolado do código de validação e o controlador, não há nenhuma maneira de modificar o código de acesso de dados em seu aplicativo sem modificar o outro código que não está diretamente relacionado ao acesso a dados.

Quando um aplicativo está acoplado livremente, por outro lado, você pode fazer alterações em uma parte de um aplicativo sem tocar em outras partes de um aplicativo. Por exemplo, você pode alternar a tecnologias de acesso a dados sem modificar sua lógica de validação ou controlador.

Nesta iteração, podemos tirar proveito dos diversos padrões de design de software que nos permitem refatorar nosso aplicativo Contact Manager em um aplicativo mais flexível. Quando são concluídas, o Gerenciador de contatos ganhou um t fazer qualquer coisa que ele t fazer antes. No entanto, ser capazes de alterar o aplicativo mais facilmente no futuro.

> [!NOTE] 
> 
> Refatoração é o processo de reconfiguração de um aplicativo de tal forma que não perca nenhuma funcionalidade existente.


## <a name="using-the-repository-software-design-pattern"></a>Usando o padrão de Design de Software do repositório

Nossa primeira alteração é tirar proveito de um padrão de design de software chamado o padrão de repositório. Usaremos o padrão Repository para isolar o nosso código de acesso a dados do restante do nosso aplicativo.

Implementando o padrão de repositório exige que possamos concluir duas etapas a seguir:

1. Criar uma interface
2. Criar uma classe concreta que implementa a interface

Primeiro, precisamos criar uma interface que descreve todos os métodos de acesso a dados que precisamos para executar. A interface IContactManagerRepository está contida na listagem 1. Essa interface descreve cinco métodos: CreateContact(), DeleteContact(), EditContact(), GetContact e ListContacts().

**Listagem 1 - Models\IContactManagerRepositiory.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample1.cs)]

Em seguida, precisamos criar uma classe concreta que implementa a interface IContactManagerRepository. Porque estamos usando o Entity Framework da Microsoft para acessar o banco de dados, vamos criar uma nova classe chamada EntityContactManagerRepository. Essa classe está contida na listagem 2.

**Listagem 2 - Models\EntityContactManagerRepository.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample2.cs)]

Observe que a classe de EntityContactManagerRepository implementa a interface IContactManagerRepository. A classe implementa todas as cinco dos métodos descritos por essa interface.

Você talvez esteja se perguntando por que precisamos se preocupar com uma interface. Por que precisamos criar uma interface e uma classe que implementa a ele?

Com uma exceção, o restante de nosso aplicativo irá interagir com a interface e não a classe concreta. Em vez de chamar os métodos expostos pela classe EntityContactManagerRepository, podemos chamar os métodos expostos pela interface IContactManagerRepository.

Dessa forma, podemos implementar a interface com uma nova classe sem precisar modificar o restante de nosso aplicativo. Por exemplo, em uma data futura, queremos implementar uma classe DataServicesContactManagerRepository que implementa a interface IContactManagerRepository. A classe DataServicesContactManagerRepository pode usar o ADO.NET Data Services para acessar um banco de dados, em vez da Microsoft Entity Framework.

Se o nosso código de aplicativo é programado com base na interface IContactManagerRepository em vez da classe EntityContactManagerRepository concreta, em seguida, é possível alternar classes concretas sem modificar o restante do nosso código. Por exemplo, é possível alternar da classe EntityContactManagerRepository para a classe DataServicesContactManagerRepository sem modificar nossa lógica de validação ou acesso de dados.

Programação em relação a interfaces (abstrações) em vez de classes concretas faz com que nosso aplicativo mais resiliente a alterar.

> [!NOTE] 
> 
> Você pode criar rapidamente uma interface de uma classe concreta dentro do Visual Studio selecionando a opção de menu Refatorar, extrair Interface. Por exemplo, você pode criar a classe EntityContactManagerRepository primeiro e, em seguida, usar extrair Interface para gerar a interface IContactManagerRepository automaticamente.


## <a name="using-the-dependency-injection-software-design-pattern"></a>Usando o padrão de Design de Software de injeção de dependência

Agora que podemos ter migrado nosso código de acesso a dados para uma classe separada de repositório, precisamos modificar nosso controlador de contato para usar essa classe. Faremos a vantagem de um padrão de design de software chamado injeção de dependência para usar a classe de repositório no nosso controlador.

O controlador de contato modificado está contido na listagem 3.

**Listagem 3 - Controllers\ContactController.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample3.cs)]

Observe que o controlador de contato na listagem 3 tem dois construtores. O primeiro construtor passa uma instância concreta da interface IContactManagerRepository para o segundo construtor. Os usos de classe do controlador de contato *injeção de dependência de construtor*.

Um e somente local que a classe EntityContactManagerRepository é usada é no construtor primeiro. O restante da classe usa a interface de IContactManagerRepository em vez da classe EntityContactManagerRepository concreta.

Isso torna fácil alternar implementações da classe IContactManagerRepository no futuro. Se você quiser usar a classe DataServicesContactRepository em vez da classe EntityContactManagerRepository, basta modificar o primeiro construtor.

Injeção de dependência de construtor também torna a classe de controlador de contato muito testável. Nos testes de unidade, você pode instanciar o controlador de contato, passando uma implementação fictícia da classe IContactManagerRepository. Esse recurso de injeção de dependência será muito importante para nós na próxima iteração, quando criamos testes de unidade para o aplicativo Gerenciador de contatos.

> [!NOTE] 
> 
> Se você quiser desacoplar completamente a classe de controlador de contato de uma implementação específica da interface IContactManagerRepository, em seguida, você pode tirar proveito de uma estrutura que dá suporte à injeção de dependência, como StructureMap ou da Microsoft Entity Framework (MEF). Ao tirar proveito de uma estrutura de injeção de dependência, você nunca precisa para se referir a uma classe concreta em seu código.


## <a name="creating-a-service-layer"></a>Criando uma camada de serviço

Você deve ter notado que nossa lógica de validação ainda misturada com nossa lógica do controlador na classe do controlador de modificada na listagem 3. Pelo mesmo motivo que é uma boa ideia isolar nossa lógica de acesso a dados, é uma boa ideia isolar nossa lógica de validação.

Para corrigir esse problema, podemos criar uma separada [ *camada de serviço*](http://martinfowler.com/eaaCatalog/serviceLayer.html). A camada de serviço é uma camada separada que podemos inserir entre nosso controlador e as classes de repositório. A camada de serviço contém nossa lógica de negócios, incluindo toda nossa lógica de validação.

O ContactManagerService está contido na listagem 4. Ele contém a lógica de validação da classe de controlador do contato.

**Listagem 4 - Models\ContactManagerService.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample4.cs)]

Observe que o construtor para o ContactManagerService requer um ValidationDictionary. A camada de serviço se comunica com a camada de controlador por meio deste ValidationDictionary. Discutiremos o ValidationDictionary detalhadamente na seção a seguir quando abordarmos o padrão decorador.

Além disso, observe que o ContactManagerService implementa a interface de IContactManagerService. Você sempre deve se esforçar para programar em interfaces em vez de classes concretas. Outras classes no aplicativo Gerenciador de contatos não interagem com a classe ContactManagerService diretamente. Em vez disso, com uma exceção, o restante do aplicativo Gerenciador de contatos é programado com base na interface IContactManagerService.

A interface IContactManagerService está contida na listagem 5.

**Listagem 5 - Models\IContactManagerService.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample5.cs)]

A classe de controlador de contato modificada está contida na listagem 6. Observe que o controlador de contato não interage com o repositório ContactManager. Em vez disso, o controlador de contato interage com o serviço ContactManager. Cada camada é isolada tanto quanto possível de outras camadas.

**Listagem 6 - Controllers\ContactController.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample6.cs)]

Nosso aplicativo não executa afoul do princípio da responsabilidade única (SRP). O controlador de contato na listagem 6 foi eliminado de toda responsabilidade diferente de controlar o fluxo de execução do aplicativo. Toda a lógica de validação foi removida do controlador de contato e enviados por push para a camada de serviço. Toda a lógica de banco de dados foi enviado para a camada de repositório.

## <a name="using-the-decorator-pattern"></a>Usando o padrão decorador

Queremos desacoplar completamente nossa camada de serviço de camada nosso controlador. Em princípio, podemos deve ser capazes de compilar nossa camada de serviço em um assembly separado da nossa camada de controlador sem a necessidade de adicionar uma referência ao nosso aplicativo MVC.

No entanto, nossa camada de serviço precisa ser capaz de passar mensagens de erro de validação de volta para a camada de controlador. Como podemos habilitar a camada de serviço para se comunicar mensagens de erro de validação sem acoplamento o controlador e a camada de serviço? Podemos pode tirar proveito de um padrão de design de software chamado a [padrão decorador](http://en.wikipedia.org/wiki/Decorator_pattern).

Um controlador usa um ModelStateDictionary chamada ModelState para representar erros de validação. Portanto, você pode ficar tentado a passar ModelState da camada de controlador para a camada de serviço. No entanto, usar ModelState na camada de serviço tornaria sua camada de serviço dependente de um recurso de estrutura do ASP.NET MVC. Isso seria muito ruim porque, um dia, você talvez queira usar a camada de serviço com um aplicativo do WPF em vez de um aplicativo ASP.NET MVC. Nesse caso, você não explicava t deseja fazer referência a estrutura ASP.NET MVC para usar a classe ModelStateDictionary.

O padrão decorador permite que você encapsule uma classe existente em uma nova classe para implementar uma interface. Nosso projeto Contact Manager inclui a classe de ModelStateWrapper contida na listagem 7. A classe ModelStateWrapper implementa a interface na listagem 8.

**Listagem 7 - Models\Validation\ModelStateWrapper.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample7.cs)]

**Listagem 8 - Models\Validation\IValidationDictionary.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample8.cs)]

Se você der uma olhada Fechar na listagem 5, em seguida, você verá que a camada de serviço ContactManager usa a interface IValidationDictionary exclusivamente. O serviço ContactManager não é dependente da classe ModelStateDictionary. Quando o controlador de contato cria o serviço ContactManager, o controlador encapsula seu ModelState como este:

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample9.cs)]

## <a name="summary"></a>Resumo

Nesta iteração, não possível adicionar nenhuma nova funcionalidade ao aplicativo Gerenciador de contatos. O objetivo dessa iteração era refatorar o aplicativo Contact Manager forma que é mais fácil de manter e modificar.

Primeiro, implementamos o padrão de design de software do repositório. Migramos todo o código de acesso de dados para uma classe de repositório ContactManager separada.

Podemos também isolados nossa lógica de validação de nossa lógica do controlador. Nós criamos uma camada de serviço separado que contém todo o nosso código de validação. A camada de controlador interage com a camada de serviço e a camada de serviço interage com a camada de repositório.

Quando criamos a camada de serviço, aproveitamos o padrão decorador para isolar ModelState da nossa camada de serviço. Nossa camada de serviço, podemos programado com base na interface IValidationDictionary em vez de ModelState.

Por fim, aproveitamos um padrão de design de software denominado o padrão de injeção de dependência. Esse padrão nos permite programar em interfaces (abstrações) em vez de classes concretas. Implementando o padrão de design de injeção de dependência também torna o nosso código mais testável. Na próxima iteração, podemos adicionar testes de unidade para nosso projeto.

> [!div class="step-by-step"]
> [Anterior](iteration-3-add-form-validation-cs.md)
> [Próximo](iteration-5-create-unit-tests-cs.md)
