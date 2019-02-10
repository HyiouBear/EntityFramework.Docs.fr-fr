---
title: Journalisation - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
uid: core/miscellaneous/logging
ms.openlocfilehash: 0a996403afdbe076b1690c98eeb305b40c4d1f4a
ms.sourcegitcommit: 109a16478de498b65717a6e09be243647e217fb3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2019
ms.locfileid: "55985572"
---
# <a name="logging"></a><span data-ttu-id="f3b53-102">Journalisation</span><span class="sxs-lookup"><span data-stu-id="f3b53-102">Logging</span></span>

> [!TIP]  
> <span data-ttu-id="f3b53-103">Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="f3b53-103">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) on GitHub.</span></span>

## <a name="aspnet-core-applications"></a><span data-ttu-id="f3b53-104">Applications ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f3b53-104">ASP.NET Core applications</span></span>

<span data-ttu-id="f3b53-105">EF Core s’intègre automatiquement avec les mécanismes de journalisation d’ASP.NET Core chaque fois que `AddDbContext` ou `AddDbContextPool` est utilisé.</span><span class="sxs-lookup"><span data-stu-id="f3b53-105">EF Core integrates automatically with the logging mechanisms of ASP.NET Core whenever `AddDbContext` or `AddDbContextPool` is used.</span></span> <span data-ttu-id="f3b53-106">Par conséquent, lorsque vous utilisez ASP.NET Core, journalisation doit être configurée comme décrit dans la [documentation ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="f3b53-106">Therefore, when using ASP.NET Core, logging should be configured as described in the [ASP.NET Core documentation](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>

## <a name="other-applications"></a><span data-ttu-id="f3b53-107">Autres applications</span><span class="sxs-lookup"><span data-stu-id="f3b53-107">Other applications</span></span>

<span data-ttu-id="f3b53-108">EF Core journalisation actuellement nécessite un ILoggerFactory qui est lui-même configuré avec un ou plusieurs ILoggerProvider.</span><span class="sxs-lookup"><span data-stu-id="f3b53-108">EF Core logging currently requires an ILoggerFactory which is itself configured with one or more ILoggerProvider.</span></span> <span data-ttu-id="f3b53-109">Fournisseurs courants sont fournis dans les packages suivants :</span><span class="sxs-lookup"><span data-stu-id="f3b53-109">Common providers are shipped in the following packages:</span></span>

* <span data-ttu-id="f3b53-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): Un journal de console simple.</span><span class="sxs-lookup"><span data-stu-id="f3b53-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): A simple console logger.</span></span>
* <span data-ttu-id="f3b53-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Prend en charge les journaux de diagnostic des Services d’application Azure et les fonctionnalités de « Log stream ».</span><span class="sxs-lookup"><span data-stu-id="f3b53-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Supports Azure App Services 'Diagnostics logs' and 'Log stream' features.</span></span>
* <span data-ttu-id="f3b53-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Journaux pour une analyse de débogueur à l’aide de System.Diagnostics.Debug.WriteLine().</span><span class="sxs-lookup"><span data-stu-id="f3b53-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Logs to a debugger monitor using System.Diagnostics.Debug.WriteLine().</span></span>
* <span data-ttu-id="f3b53-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Journaux pour le journal des événements Windows.</span><span class="sxs-lookup"><span data-stu-id="f3b53-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Logs to Windows Event Log.</span></span>
* <span data-ttu-id="f3b53-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Prend en charge/EventListener EventSource.</span><span class="sxs-lookup"><span data-stu-id="f3b53-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Supports EventSource/EventListener.</span></span>
* <span data-ttu-id="f3b53-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Journaux pour un écouteur de suivi à l’aide de System.Diagnostics.TraceSource.TraceEvent().</span><span class="sxs-lookup"><span data-stu-id="f3b53-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Logs to a trace listener using System.Diagnostics.TraceSource.TraceEvent().</span></span>

> [!NOTE]
> <span data-ttu-id="f3b53-116">Le code suivant exemple utilise un `ConsoleLoggerProvider` constructeur qui a été obsolète dans la version 2.2.</span><span class="sxs-lookup"><span data-stu-id="f3b53-116">The following code sample uses a `ConsoleLoggerProvider` constructor that has been obsoleted in version 2.2.</span></span> <span data-ttu-id="f3b53-117">Remplacements appropriés pour les API de journalisation obsolète seront disponibles dans la version 3.0.</span><span class="sxs-lookup"><span data-stu-id="f3b53-117">Proper replacements for obsolete logging APIs will be available in version 3.0.</span></span> <span data-ttu-id="f3b53-118">En attendant, il est possible sans ignorer et supprimer les avertissements.</span><span class="sxs-lookup"><span data-stu-id="f3b53-118">In the meantime, it is safe to ignore and suppress the warnings.</span></span>

<span data-ttu-id="f3b53-119">Après avoir installé les packages appropriés, l’application doit créer une instance de singleton/globale d’un LoggerFactory.</span><span class="sxs-lookup"><span data-stu-id="f3b53-119">After installing the appropriate package(s), the application should create a singleton/global instance of a LoggerFactory.</span></span> <span data-ttu-id="f3b53-120">Par exemple, utilisez le journal de console :</span><span class="sxs-lookup"><span data-stu-id="f3b53-120">For example, using the console logger:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

<span data-ttu-id="f3b53-121">Cette instance de singleton/globale doit être inscrits avec EF Core sur le `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="f3b53-121">This singleton/global instance should then be registered with EF Core on the `DbContextOptionsBuilder`.</span></span> <span data-ttu-id="f3b53-122">Exemple :</span><span class="sxs-lookup"><span data-stu-id="f3b53-122">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> <span data-ttu-id="f3b53-123">Il est très important que les applications ne créent pas une nouvelle instance de ILoggerFactory pour chaque instance de contexte.</span><span class="sxs-lookup"><span data-stu-id="f3b53-123">It is very important that applications do not create a new ILoggerFactory instance for each context instance.</span></span> <span data-ttu-id="f3b53-124">Cela entraîne une fuite de mémoire et des performances médiocres.</span><span class="sxs-lookup"><span data-stu-id="f3b53-124">Doing so will result in a memory leak and poor performance.</span></span>

## <a name="filtering-what-is-logged"></a><span data-ttu-id="f3b53-125">Filtrage de ce qui est enregistré</span><span class="sxs-lookup"><span data-stu-id="f3b53-125">Filtering what is logged</span></span>

> [!NOTE]
> <span data-ttu-id="f3b53-126">Le code suivant exemple utilise un `ConsoleLoggerProvider` constructeur qui a été obsolète dans la version 2.2.</span><span class="sxs-lookup"><span data-stu-id="f3b53-126">The following code sample uses a `ConsoleLoggerProvider` constructor that has been obsoleted in version 2.2.</span></span> <span data-ttu-id="f3b53-127">Remplacements appropriés pour les API de journalisation obsolète seront disponibles dans la version 3.0.</span><span class="sxs-lookup"><span data-stu-id="f3b53-127">Proper replacements for obsolete logging APIs will be available in version 3.0.</span></span> <span data-ttu-id="f3b53-128">En attendant, il est possible sans ignorer et supprimer les avertissements.</span><span class="sxs-lookup"><span data-stu-id="f3b53-128">In the meantime, it is safe to ignore and suppress the warnings.</span></span>

<span data-ttu-id="f3b53-129">Pour filtrer ce qui est enregistré, le plus simple consiste à configurer lors de l’inscription de l’ILoggerProvider.</span><span class="sxs-lookup"><span data-stu-id="f3b53-129">The easiest way to filter what is logged is to configure it when registering the ILoggerProvider.</span></span> <span data-ttu-id="f3b53-130">Exemple :</span><span class="sxs-lookup"><span data-stu-id="f3b53-130">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

<span data-ttu-id="f3b53-131">Dans cet exemple, le journal est filtré pour retourner uniquement les messages :</span><span class="sxs-lookup"><span data-stu-id="f3b53-131">In this example, the log is filtered to return only messages:</span></span>
 * <span data-ttu-id="f3b53-132">dans la catégorie « Microsoft.EntityFrameworkCore.Database.Command »</span><span class="sxs-lookup"><span data-stu-id="f3b53-132">in the 'Microsoft.EntityFrameworkCore.Database.Command' category</span></span>
 * <span data-ttu-id="f3b53-133">au niveau des « Informations »</span><span class="sxs-lookup"><span data-stu-id="f3b53-133">at the 'Information' level</span></span>

<span data-ttu-id="f3b53-134">Pour EF Core, les catégories de l’enregistreur d’événements sont définies dans le `DbLoggerCategory` résoudre les classe pour le rendre plus faciles à trouver les catégories, mais ces chaînes simples.</span><span class="sxs-lookup"><span data-stu-id="f3b53-134">For EF Core, logger categories are defined in the `DbLoggerCategory` class to make it easy to find categories, but these resolve to simple strings.</span></span>

<span data-ttu-id="f3b53-135">Vous trouverez plus d’informations sur l’infrastructure sous-jacente de la journalisation dans le [documentation sur la journalisation ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="f3b53-135">More details on the underlying logging infrastructure can be found in the [ASP.NET Core logging documentation](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>
