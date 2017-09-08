---
title: "Introdução à proteção de dados"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 4542cd37-b47c-454c-be19-d1b5810d67fe
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/introduction
ms.openlocfilehash: bcf1ce5a272a374c9605e50dee5c5fb27305527d
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2017
---
# <a name="introduction-to-data-protection"></a>Introdução à proteção de dados

Aplicativos da Web geralmente necessário armazenar dados confidenciais. O Windows fornece DPAPI para aplicativos de desktop, mas isso é inadequado para aplicativos da web. A pilha de proteção de dados do ASP.NET Core fornecem uma API de criptografia simple e fácil de usar um desenvolvedor pode usar para proteger dados, incluindo gerenciamento de chave e rotação.

A pilha de proteção de dados do ASP.NET Core foi projetada para servir como a substituição de longo prazo para o <machineKey> elemento no ASP.NET 1. x - 4. x. Ele foi projetado para resolver muitos dos problemas da pilha de criptografia antigo enquanto fornece uma solução para a maioria dos casos de uso, os aplicativos modernos têm probabilidade de encontrar.

## <a name="problem-statement"></a>Declaração do problema

A declaração do problema geral pode ser declarada em um único sentença sucintamente: necessário manter informações confiáveis para recuperação posterior, mas não confia que o mecanismo de persistência. Em termos de web, isso pode ser escrito como "Preciso ir e voltar estado confiável por meio de um cliente não confiável."

O exemplo canônico disso é um cookie de autenticação ou portador token. O servidor gera um "Estou Groot e ter permissões de xyz" token e encaminha-lo ao cliente. No futuro, o cliente apresentará esse token para o servidor, mas o servidor precisa de algum tipo de garantia de que o cliente não foi forjada o token. Portanto, o primeiro requisito: autenticidade (também conhecido como integridade e à prova de adulteração).

Como o estado persistente é confiável pelo servidor, estimamos que esse estado pode conter informações específicas para o ambiente operacional. Isso pode ser na forma de um caminho de arquivo, uma permissão, um identificador ou outra referência indireta ou alguma outra parte de dados específico do servidor. Essas informações, geralmente, não devem ser reveladas para um cliente não confiável. Portanto, o segundo requisito: confidencialidade.

Finalmente, desde que os aplicativos modernos são componentizados, o que vimos é que componentes individuais queira tirar proveito do sistema sem considerar outros componentes no sistema. Por exemplo, se um componente de token de portador está usando esta pilha, ele deve operar sem interferência de um mecanismo de anti-CSRF que também pode usar a mesma pilha. Portanto, o requisito de final: isolamento.

Podemos fornecer mais restrições para restringir o escopo dos nossos requisitos. Vamos supor que todos os serviços que operam com o criptográfico são igualmente confiáveis e que os dados não precisam ser gerado ou consumido fora os serviços em nosso controle direto. Além disso, é necessário que as operações são tão rápidas quanto possível, desde que cada solicitação ao serviço web pode percorrer o criptográfico uma ou mais vezes. Isso torna a criptografia simétrica ideal para o nosso cenário, e nós pode desconto criptografia assimétrica até como uma hora que é necessário.

## <a name="design-philosophy"></a>Filosofia de design

Começamos identificando problemas com a pilha existente. Depois que tivemos que, podemos pesquisadas o cenário de soluções existentes e concluiu que nenhuma solução existente tinha muito dos recursos que são procurados. Em seguida, projetamos uma solução baseada em vários princípios.

* O sistema deve oferecer simplicidade de configuração. O ideal é o sistema seria nenhuma configuração e os desenvolvedores foi atingido o início em execução. Em situações em que os desenvolvedores precisam configurar um aspecto específico (como o repositório de chaves), consideração deve ser fornecida a fazer essas configurações específicas simples.

* Oferece uma API simples do voltado ao consumidor. As APIs deve ser fácil de usar corretamente e difícil de usar incorretamente.

* Os desenvolvedores não devem conhecer os princípios de gerenciamento de chaves. O sistema deve lidar com a seleção do algoritmo e a vida útil da chave em nome do desenvolvedor. Idealmente o desenvolvedor nunca mesmo deve ter acesso ao material de chave bruto.

* As chaves devem ser protegidas em repouso quando possível. O sistema deve descobrir um mecanismo de proteção padrão apropriado e aplicá-lo automaticamente.

Esses princípios em mente, desenvolvemos um simples, [fácil de usar](using-data-protection.md) pilha de proteção de dados.

A proteção de dados do ASP.NET Core APIs não são principalmente para persistência indefinida de cargas confidenciais. Outras tecnologias, como [Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) e [Azure Rights Management](https://technet.microsoft.com/library/jj585024.aspx) são mais adequados para o cenário de armazenamento indefinido, e eles têm recursos de gerenciamento de chave forte de forma correspondente. Dito isso, não há nada proibindo um desenvolvedor usa as APIs de proteção de dados ASP.NET Core para proteção de longo prazo de dados confidenciais.

## <a name="audience"></a>Público-alvo

O sistema de proteção de dados é dividido em cinco pacotes principais. Vários aspectos dessas APIs três público principal; de destino

1. O [visão geral de APIs do consumidor](consumer-apis/overview.md) os desenvolvedores de aplicativo e do framework de destino.

   "Quero saber mais sobre como funciona a pilha de ou sobre como ele está configurado. Simplesmente desejo executar alguma operação no simples assim uma maneira possível com alta probabilidade de usar as APIs com êxito."

2. O [APIs de configuração](configuration/overview.md) os desenvolvedores de aplicativos e os administradores do sistema de destino.

   "Eu preciso informar o sistema de proteção de dados que o meu ambiente requer caminhos não padrão ou as configurações".

3. Os desenvolvedores de destino APIs de extensibilidade responsável pela implementação de política personalizada. O uso dessas APIs seria limitado a situações raras e experientes, os desenvolvedores com reconhecimento de segurança.

   "Preciso substituir um componente inteiro dentro do sistema porque tenho requisitos comportamentais realmente exclusivos. Desejo saber uncommonly usado partes da superfície de API para criar um plug-in que atende aos meus requisitos."

## <a name="package-layout"></a>Layout do pacote

A pilha de proteção de dados consiste em cinco pacotes.

* Microsoft.AspNetCore.DataProtection.Abstractions contém as interfaces IDataProtectionProvider e IDataProtector básicas. Ele também contém métodos de extensão úteis que podem ajudá-lo a trabalhar com esses tipos (por exemplo, sobrecargas de IDataProtector.Protect). Consulte a seção de interfaces de consumidor para obter mais informações. Se alguém é responsável por criar uma instância do sistema de proteção de dados e as APIs simplesmente está consumindo, você desejará Microsoft.AspNetCore.DataProtection.Abstractions de referência.

* Microsoft.AspNetCore.DataProtection contém a implementação de núcleo do sistema proteção de dados, incluindo as principais operações criptográficas, gerenciamento de chaves, configuração e extensibilidade. Se você é responsável para instanciar o sistema de proteção de dados (por exemplo, adicioná-lo para um IServiceCollection) ou modificar ou estender seu comportamento, você desejará Microsoft.AspNetCore.DataProtection de referência.

* Microsoft.AspNetCore.DataProtection.Extensions contém APIs adicionais que os desenvolvedores podem ser úteis, mas que não pertencem no pacote principal. Por exemplo, este pacote contém uma API simples "instanciar o sistema apontando para um diretório de armazenamento de chave específico com nenhuma configuração de injeção de dependência" (mais informações). Ele também contém métodos de extensão para limitar o tempo de vida das cargas protegidos (mais informações).

* Microsoft.AspNetCore.DataProtection.SystemWeb pode ser instalado em um aplicativo de 4. x ASP.NET existente para redirecionar seu <machineKey> operações em vez disso, usar a nova pilha de proteção de dados. Consulte [compatibilidade](compatibility/replacing-machinekey.md#compatibility-replacing-machinekey) para obter mais informações.

* Microsoft.AspNetCore.Cryptography.KeyDerivation fornece uma implementação da rotina de hash de senha do PBKDF2 e podem ser usadas por sistemas que precisam lidar com as senhas de usuário com segurança. Consulte [senha hash](consumer-apis/password-hashing.md) para obter mais informações.
