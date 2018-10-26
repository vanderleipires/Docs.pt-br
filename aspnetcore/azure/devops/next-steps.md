---
title: DevOps com o ASP.NET Core e o Azure | Próximas etapas
author: CamSoper
description: Um guia que fornece orientação de ponta a ponta sobre a criação de um pipeline de DevOps para um aplicativo ASP.NET Core hospedado no Azure.
ms.author: casoper
ms.custom: mvc
ms.date: 10/24/2018
uid: azure/devops/next-steps
ms.openlocfilehash: b82e7251b507f8d141930673d50722cfaa576db5
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50089871"
---
# <a name="next-steps"></a><span data-ttu-id="9f044-103">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="9f044-103">Next steps</span></span>

<span data-ttu-id="9f044-104">Neste guia, você criou um pipeline de DevOps para um aplicativo de exemplo do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9f044-104">In this guide, you created a DevOps pipeline for an ASP.NET Core sample app.</span></span> <span data-ttu-id="9f044-105">Parabéns!</span><span class="sxs-lookup"><span data-stu-id="9f044-105">Congratulations!</span></span> <span data-ttu-id="9f044-106">Esperamos que você tenha gostado de aprendizado publicar aplicativos web ASP.NET Core no serviço de aplicativo do Azure e automatizar a integração contínua das alterações.</span><span class="sxs-lookup"><span data-stu-id="9f044-106">We hope you enjoyed learning to publish ASP.NET Core web apps to Azure App Service and automate the continuous integration of changes.</span></span>

<span data-ttu-id="9f044-107">Além de hospedagem na web e DevOps, o Azure tem uma ampla gama de serviços de plataforma-como um serviço (PaaS) útil para desenvolvedores do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9f044-107">Beyond web hosting and DevOps, Azure has a wide array of Platform-as-a-Service (PaaS) services useful to ASP.NET Core developers.</span></span> <span data-ttu-id="9f044-108">Esta seção fornece uma visão geral de alguns dos serviços mais comumente usados.</span><span class="sxs-lookup"><span data-stu-id="9f044-108">This section gives a brief overview of some of the most commonly used services.</span></span>

## <a name="storage-and-databases"></a><span data-ttu-id="9f044-109">Armazenamento e bancos de dados</span><span class="sxs-lookup"><span data-stu-id="9f044-109">Storage and databases</span></span>

<span data-ttu-id="9f044-110">[Cache redis](/azure/redis-cache/) está disponível como um serviço de cache de dados de alta taxa de transferência e baixa latência.</span><span class="sxs-lookup"><span data-stu-id="9f044-110">[Redis Cache](/azure/redis-cache/) is high-throughput, low-latency data caching available as a service.</span></span> <span data-ttu-id="9f044-111">Ele pode ser usado para armazenamento em cache de saída de página, reduzindo as solicitações de banco de dados e fornecendo o estado de sessão do ASP.NET Core em várias instâncias de um aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9f044-111">It can be used for caching page output, reducing database requests, and providing ASP.NET Core session state across multiple instances of an app.</span></span>

<span data-ttu-id="9f044-112">[O armazenamento do Azure](/azure/storage/) é armazenamento em nuvem altamente escalonável do Azure.</span><span class="sxs-lookup"><span data-stu-id="9f044-112">[Azure Storage](/azure/storage/) is Azure's massively scalable cloud storage.</span></span> <span data-ttu-id="9f044-113">Os desenvolvedores podem aproveitar [armazenamento de filas](/azure/storage/queues/storage-queues-introduction) para enfileiramento de mensagens confiável, e [armazenamento de tabelas](/azure/storage/tables/table-storage-overview) é um repositório de chave-valor NoSQL projetado para rápido desenvolvimento usando conjuntos de dados grandes e semi-estruturados.</span><span class="sxs-lookup"><span data-stu-id="9f044-113">Developers can take advantage of [Queue Storage](/azure/storage/queues/storage-queues-introduction) for reliable message queuing, and [Table Storage](/azure/storage/tables/table-storage-overview) is a NoSQL key-value store designed for rapid development using massive, semi-structured data sets.</span></span>

<span data-ttu-id="9f044-114">[Banco de dados SQL do Azure](/azure/sql-database/) fornece a funcionalidade de banco de dados relacional conhecida como um serviço usando o mecanismo do Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="9f044-114">[Azure SQL Database](/azure/sql-database/) provides familiar relational database functionality as a service using the Microsoft SQL Server Engine.</span></span>

<span data-ttu-id="9f044-115">[O cosmos DB](/azure/cosmos-db/) serviço de banco de dados NoSQL multimodelo, distribuído globalmente.</span><span class="sxs-lookup"><span data-stu-id="9f044-115">[Cosmos DB](/azure/cosmos-db/) globally distributed, multi-model NoSQL database service.</span></span> <span data-ttu-id="9f044-116">Várias APIs estão disponíveis, incluindo MongoDB, Cassandra e API do SQL (anteriormente chamado de DocumentDB).</span><span class="sxs-lookup"><span data-stu-id="9f044-116">Multiple APIs are available, including SQL API (formerly called DocumentDB), Cassandra, and MongoDB.</span></span>

## <a name="identity"></a><span data-ttu-id="9f044-117">Identidade</span><span class="sxs-lookup"><span data-stu-id="9f044-117">Identity</span></span>

<span data-ttu-id="9f044-118">[O Azure Active Directory](/azure/active-directory/) e [Azure Active Directory B2C](/azure/active-directory-b2c/) são ambos os serviços de identidade.</span><span class="sxs-lookup"><span data-stu-id="9f044-118">[Azure Active Directory](/azure/active-directory/) and [Azure Active Directory B2C](/azure/active-directory-b2c/) are both identity services.</span></span> <span data-ttu-id="9f044-119">O Azure Active Directory foi projetado para cenários empresariais e permite a colaboração do Azure AD B2B (business-to-business), enquanto o Azure Active Directory B2C é pretendidos cenários de negócios para o cliente, incluindo rede social entrar.</span><span class="sxs-lookup"><span data-stu-id="9f044-119">Azure Active Directory is designed for enterprise scenarios and enables Azure AD B2B (business-to-business) collaboration, while Azure Active Directory B2C is intended business-to-customer scenarios, including social network sign-in.</span></span>

## <a name="mobile"></a><span data-ttu-id="9f044-120">Celular</span><span class="sxs-lookup"><span data-stu-id="9f044-120">Mobile</span></span>

<span data-ttu-id="9f044-121">[Os Hubs de notificação](/azure/notification-hubs/) é um mecanismo de notificação por push multiplataforma e dimensionável para enviar rapidamente milhões de mensagens para aplicativos em execução em vários tipos de dispositivos.</span><span class="sxs-lookup"><span data-stu-id="9f044-121">[Notification Hubs](/azure/notification-hubs/) is a multi-platform, scalable push-notification engine to quickly send millions of messages to apps running on various types of devices.</span></span>

## <a name="web-infrastructure"></a><span data-ttu-id="9f044-122">Infraestrutura da Web</span><span class="sxs-lookup"><span data-stu-id="9f044-122">Web infrastructure</span></span>

<span data-ttu-id="9f044-123">[O serviço de contêiner do Azure](/azure/aks/) gerencia seu ambiente Kubernetes hospedado, tornando rápido e fácil de implantar e gerenciar aplicativos em contêineres sem conhecimento de orquestração de contêiner.</span><span class="sxs-lookup"><span data-stu-id="9f044-123">[Azure Container Service](/azure/aks/) manages your hosted Kubernetes environment, making it quick and easy to deploy and manage containerized apps without container orchestration expertise.</span></span>

<span data-ttu-id="9f044-124">[O Azure Search](/azure/search/) é usado para criar uma solução de pesquisa empresarial sobre conteúdo privada e heterogênea.</span><span class="sxs-lookup"><span data-stu-id="9f044-124">[Azure Search](/azure/search/) is used to create an enterprise search solution over private, heterogenous content.</span></span>

<span data-ttu-id="9f044-125">[O Service Fabric](/azure/service-fabric/) é uma plataforma de sistemas distribuídos que torna mais fácil empacotar, implantar e gerenciar escalonável e confiáveis microsserviços e contêineres.</span><span class="sxs-lookup"><span data-stu-id="9f044-125">[Service Fabric](/azure/service-fabric/) is a distributed systems platform that makes it easy to package, deploy, and manage scalable and reliable microservices and containers.</span></span>
