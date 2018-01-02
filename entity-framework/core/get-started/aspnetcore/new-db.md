---
title: "Bien démarrer avec ASP.NET Core - Nouvelle base de données - EF Core"
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
ms.date: 04/07/2017
ms.topic: get-started-article
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: 7e7ecaff29e9830bf3bcf742e6a5d54e1ced24de
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2017
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a><span data-ttu-id="9c5a3-102">Bien démarrer avec EF Core sur ASP.NET Core avec une nouvelle base de données</span><span class="sxs-lookup"><span data-stu-id="9c5a3-102">Getting Started with EF Core on ASP.NET Core with a New database</span></span>

<span data-ttu-id="9c5a3-103">Dans cette procédure pas à pas, vous allez générer une application ASP.NET Core MVC exécutant l’accès aux données de base à l’aide d’Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="9c5a3-103">In this walkthrough, you will build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="9c5a3-104">Vous allez utiliser des migrations pour créer la base de données à partir de votre modèle EF Core.</span><span class="sxs-lookup"><span data-stu-id="9c5a3-104">You will use migrations to create the database from your EF Core model.</span></span> <span data-ttu-id="9c5a3-105">Consultez [Ressources supplémentaires](#additional-resources) pour accéder aux autres didacticiels sur Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="9c5a3-105">See [Additional Resources](#additional-resources) for more Entity Framework Core tutorials.</span></span>

<span data-ttu-id="9c5a3-106">Ce didacticiel requiert :</span><span class="sxs-lookup"><span data-stu-id="9c5a3-106">This tutorial requires:</span></span>
* <span data-ttu-id="9c5a3-107">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) avec les charges de travail suivantes :</span><span class="sxs-lookup"><span data-stu-id="9c5a3-107">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="9c5a3-108">**ASP.NET et développement web** (sous **Web et cloud**)</span><span class="sxs-lookup"><span data-stu-id="9c5a3-108">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="9c5a3-109">**Développement multiplateforme .NET Core** (sous **Autres ensembles d’outils**)</span><span class="sxs-lookup"><span data-stu-id="9c5a3-109">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="9c5a3-110">[Kit SDK .NET Core 2.0](https://www.microsoft.com/net/download/core)</span><span class="sxs-lookup"><span data-stu-id="9c5a3-110">[.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core).</span></span>

> [!TIP]  
> <span data-ttu-id="9c5a3-111">Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb) sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="9c5a3-111">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb) on GitHub.</span></span>

## <a name="create-a-new-project-in-visual-studio-2017"></a><span data-ttu-id="9c5a3-112">Créer un projet dans Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="9c5a3-112">Create a new project in Visual Studio 2017</span></span>

* <span data-ttu-id="9c5a3-113">**Fichier > Nouveau > Projet**</span><span class="sxs-lookup"><span data-stu-id="9c5a3-113">**File > New > Project**</span></span>
* <span data-ttu-id="9c5a3-114">Dans le menu de gauche, sélectionnez **Installé > Modèles > Visual C# > .NET Core**.</span><span class="sxs-lookup"><span data-stu-id="9c5a3-114">From the left menu select **Installed > Templates > Visual C# > .NET Core**.</span></span>
* <span data-ttu-id="9c5a3-115">Sélectionnez **Nouvelle application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="9c5a3-115">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="9c5a3-116">Entrez le nom **EFGetStarted.AspNetCore.NewDb** et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="9c5a3-116">Enter **EFGetStarted.AspNetCore.NewDb** for the name and click **OK**.</span></span>
* <span data-ttu-id="9c5a3-117">Dans la boîte de dialogue **Nouvelle application web ASP.NET Core** :</span><span class="sxs-lookup"><span data-stu-id="9c5a3-117">In the **New ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="9c5a3-118">Vérifiez que les options **.NET Core** et **ASP.NET Core 2.0** sont sélectionnées dans les listes déroulantes.</span><span class="sxs-lookup"><span data-stu-id="9c5a3-118">Ensure the options **.NET Core** and **ASP.NET Core 2.0** are selected in the drop down lists</span></span>
  * <span data-ttu-id="9c5a3-119">Sélectionnez le modèle de projet **Application web (Model-View-Controller)**.</span><span class="sxs-lookup"><span data-stu-id="9c5a3-119">Select the **Web Application (Model-View-Controller)** project template</span></span>
  * <span data-ttu-id="9c5a3-120">Vérifiez que le paramètre **Authentification** est défini sur **Aucune authentification**.</span><span class="sxs-lookup"><span data-stu-id="9c5a3-120">Ensure that **Authentication** is set to **No Authentication**</span></span>
  * <span data-ttu-id="9c5a3-121">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="9c5a3-121">Click **OK**</span></span>

<span data-ttu-id="9c5a3-122">Avertissement: si vous définissez le paramètre **Authentification** sur **Comptes d’utilisateur individuels** au lieu de **Aucune**, un modèle Entity Framework Core sera ajouté à votre projet dans `Models\IdentityModel.cs`.</span><span class="sxs-lookup"><span data-stu-id="9c5a3-122">Warning: If you use **Individual User Accounts** instead of **None** for **Authentication** then an Entity Framework Core model will be added to your project in `Models\IdentityModel.cs`.</span></span> <span data-ttu-id="9c5a3-123">À l’aide des techniques que vous allez découvrir dans cette procédure pas à pas, vous pouvez ajouter un second modèle ou étendre ce modèle existant pour qu’il puisse contenir vos classes d’entité.</span><span class="sxs-lookup"><span data-stu-id="9c5a3-123">Using the techniques you will learn in this walkthrough, you can choose to add a second model, or extend this existing model to contain your entity classes.</span></span>

## <a name="install-entity-framework-core"></a><span data-ttu-id="9c5a3-124">Installer Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="9c5a3-124">Install Entity Framework Core</span></span>

<span data-ttu-id="9c5a3-125">Installez le package pour le ou les fournisseurs de bases de données EF Core à cibler.</span><span class="sxs-lookup"><span data-stu-id="9c5a3-125">Install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="9c5a3-126">Cette procédure pas à pas utilise SQL Server.</span><span class="sxs-lookup"><span data-stu-id="9c5a3-126">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="9c5a3-127">Pour obtenir la liste des fournisseurs disponibles, consultez [Fournisseurs de bases de données](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="9c5a3-127">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="9c5a3-128">**Outils > Gestionnaire de package NuGet > Console du Gestionnaire de package**</span><span class="sxs-lookup"><span data-stu-id="9c5a3-128">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="9c5a3-129">Exécutez `Install-Package Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="9c5a3-129">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="9c5a3-130">Nous utiliserons des outils Entity Framework Core pour créer une base de données à partir de votre modèle EF Core.</span><span class="sxs-lookup"><span data-stu-id="9c5a3-130">We will be using some Entity Framework Core Tools to create a database from your EF Core model.</span></span> <span data-ttu-id="9c5a3-131">Nous installerons donc également le package d’outils :</span><span class="sxs-lookup"><span data-stu-id="9c5a3-131">So we will install the tools package as well:</span></span>

* <span data-ttu-id="9c5a3-132">Exécutez `Install-Package Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="9c5a3-132">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

<span data-ttu-id="9c5a3-133">Nous utiliserons des outils de génération de modèles automatique ASP.NET Core pour créer par la suite des contrôleurs et des vues.</span><span class="sxs-lookup"><span data-stu-id="9c5a3-133">We will be using some ASP.NET Core Scaffolding tools to create controllers and views later on.</span></span> <span data-ttu-id="9c5a3-134">Nous installerons donc également ce package de conception :</span><span class="sxs-lookup"><span data-stu-id="9c5a3-134">So we will install this design package as well:</span></span>

* <span data-ttu-id="9c5a3-135">Exécutez `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`.</span><span class="sxs-lookup"><span data-stu-id="9c5a3-135">Run `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`</span></span>

## <a name="create-the-model"></a><span data-ttu-id="9c5a3-136">Créer le modèle</span><span class="sxs-lookup"><span data-stu-id="9c5a3-136">Create the model</span></span>

<span data-ttu-id="9c5a3-137">Définissez le contexte et les classes d’entité qui composeront le modèle :</span><span class="sxs-lookup"><span data-stu-id="9c5a3-137">Define a context and entity classes that make up the model:</span></span>

* <span data-ttu-id="9c5a3-138">Cliquez avec le bouton de droite sur le dossier **Modèles** et sélectionnez **Ajouter > Classe**.</span><span class="sxs-lookup"><span data-stu-id="9c5a3-138">Right-click on the **Models** folder and select **Add > Class**.</span></span>
* <span data-ttu-id="9c5a3-139">Entrez le nom **Model.cs** et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="9c5a3-139">Enter **Model.cs** as the name and click **OK**.</span></span>
* <span data-ttu-id="9c5a3-140">Remplacez le contenu du fichier par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="9c5a3-140">Replace the contents of the file with the following code:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

<span data-ttu-id="9c5a3-141">Remarque : dans une application réelle, vous placeriez généralement chaque classe de votre modèle dans un fichier distinct.</span><span class="sxs-lookup"><span data-stu-id="9c5a3-141">Note: In a real app you would typically put each class from your model in a separate file.</span></span> <span data-ttu-id="9c5a3-142">Dans le cadre de ce didacticiel, par souci de simplicité, nous plaçons toutes les classes dans un même fichier.</span><span class="sxs-lookup"><span data-stu-id="9c5a3-142">For the sake of simplicity, we are putting all the classes in one file for this tutorial.</span></span>

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="9c5a3-143">Inscrire le contexte avec l’injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="9c5a3-143">Register your context with dependency injection</span></span>

<span data-ttu-id="9c5a3-144">Des services (tels que `BloggingContext`) sont inscrits avec l’[injection de dépendances](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="9c5a3-144">Services (such as `BloggingContext`) are registered with [dependency injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) during application startup.</span></span> <span data-ttu-id="9c5a3-145">Ces services sont affectés aux composants qui les nécessitent (par exemple, vos contrôleurs MVC) par le biais des propriétés ou paramètres du constructeur.</span><span class="sxs-lookup"><span data-stu-id="9c5a3-145">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span>

<span data-ttu-id="9c5a3-146">Afin de permettre à nos contrôleurs MVC d’utiliser `BloggingContext`, nous allons inscrire ce dernier en tant que service.</span><span class="sxs-lookup"><span data-stu-id="9c5a3-146">In order for our MVC controllers to make use of `BloggingContext` we will register it as a service.</span></span>

* <span data-ttu-id="9c5a3-147">Ouvrez **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="9c5a3-147">Open **Startup.cs**</span></span>
* <span data-ttu-id="9c5a3-148">Ajoutez les instructions `using` suivantes :</span><span class="sxs-lookup"><span data-stu-id="9c5a3-148">Add the following `using` statements:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

<span data-ttu-id="9c5a3-149">Ajoutez la méthode `AddDbContext` pour l’inscrire en tant que service :</span><span class="sxs-lookup"><span data-stu-id="9c5a3-149">Add the `AddDbContext` method to register it as a service:</span></span>

* <span data-ttu-id="9c5a3-150">Ajoutez le code suivant à la méthode `ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="9c5a3-150">Add the following code to the `ConfigureServices` method:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

<span data-ttu-id="9c5a3-151">Remarque : une application réelle placerait généralement la chaîne de connexion dans un fichier de configuration.</span><span class="sxs-lookup"><span data-stu-id="9c5a3-151">Note: A real app would gennerally put the connection string in a configuration file.</span></span> <span data-ttu-id="9c5a3-152">Par souci de simplicité, nous allons la définir dans le code.</span><span class="sxs-lookup"><span data-stu-id="9c5a3-152">For the sake of simplicity, we are defining it in code.</span></span> <span data-ttu-id="9c5a3-153">Pour plus d’informations, consultez [Chaînes de connexion](../../miscellaneous/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="9c5a3-153">See [Connection Strings](../../miscellaneous/connection-strings.md) for more information.</span></span>

## <a name="create-your-database"></a><span data-ttu-id="9c5a3-154">Créer la base de données</span><span class="sxs-lookup"><span data-stu-id="9c5a3-154">Create your database</span></span>

<span data-ttu-id="9c5a3-155">Une fois que vous avez un modèle, vous pouvez utiliser des [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) pour créer une base de données.</span><span class="sxs-lookup"><span data-stu-id="9c5a3-155">Once you have a model, you can use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="9c5a3-156">Ouvrez la console du Gestionnaire de package (PMC) :</span><span class="sxs-lookup"><span data-stu-id="9c5a3-156">Open the PMC:</span></span>

  <span data-ttu-id="9c5a3-157">**Outils –> Gestionnaire de package NuGet –> Console du Gestionnaire de package**</span><span class="sxs-lookup"><span data-stu-id="9c5a3-157">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>
* <span data-ttu-id="9c5a3-158">Exécutez `Add-Migration InitialCreate` pour générer automatiquement un modèle de migration pour créer l’ensemble initial de tables de votre modèle.</span><span class="sxs-lookup"><span data-stu-id="9c5a3-158">Run `Add-Migration InitialCreate` to scaffold a migration to create the initial set of tables for your model.</span></span> <span data-ttu-id="9c5a3-159">Si vous recevez l’erreur `The term 'add-migration' is not recognized as the name of a cmdlet`, fermez et rouvrez Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9c5a3-159">If you receive an error stating `The term 'add-migration' is not recognized as the name of a cmdlet`, close and reopen Visual Studio.</span></span>
* <span data-ttu-id="9c5a3-160">Exécutez `Update-Database` pour appliquer la nouvelle migration à la base de données.</span><span class="sxs-lookup"><span data-stu-id="9c5a3-160">Run `Update-Database` to apply the new migration to the database.</span></span> <span data-ttu-id="9c5a3-161">Cette commande crée la base de données avant d’appliquer des migrations.</span><span class="sxs-lookup"><span data-stu-id="9c5a3-161">This command creates the database before applying migrations.</span></span>

## <a name="create-a-controller"></a><span data-ttu-id="9c5a3-162">Créer un contrôleur</span><span class="sxs-lookup"><span data-stu-id="9c5a3-162">Create a controller</span></span>

<span data-ttu-id="9c5a3-163">Activez la génération de modèles automatique dans le projet :</span><span class="sxs-lookup"><span data-stu-id="9c5a3-163">Enable scaffolding in the project:</span></span>

* <span data-ttu-id="9c5a3-164">Dans l’**Explorateur de solutions**, cliquez avec le bouton de droite sur le dossier **Contrôleurs** et sélectionnez **Ajouter > Contrôleur**.</span><span class="sxs-lookup"><span data-stu-id="9c5a3-164">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add > Controller**.</span></span>
* <span data-ttu-id="9c5a3-165">Sélectionnez **Dépendances minimales** et cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="9c5a3-165">Select **Minimal Dependencies** and click **Add**.</span></span>
* <span data-ttu-id="9c5a3-166">Vous pouvez ignorer ou supprimer le fichier *ScaffoldingReadMe.txt*.</span><span class="sxs-lookup"><span data-stu-id="9c5a3-166">You can ignore or delete the *ScaffoldingReadMe.txt* file.</span></span>

<span data-ttu-id="9c5a3-167">Une fois activée la génération de modèles automatique, nous pouvons générer automatiquement un modèle de contrôleur pour l’entité `Blog`.</span><span class="sxs-lookup"><span data-stu-id="9c5a3-167">Now that scaffolding is enabled, we can scaffold a controller for the `Blog` entity.</span></span>

* <span data-ttu-id="9c5a3-168">Dans l’**Explorateur de solutions**, cliquez avec le bouton de droite sur le dossier **Contrôleurs** et sélectionnez **Ajouter > Contrôleur**.</span><span class="sxs-lookup"><span data-stu-id="9c5a3-168">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add > Controller**.</span></span>
* <span data-ttu-id="9c5a3-169">Sélectionnez **Contrôleur MVC avec vues, utilisant Entity Framework** et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="9c5a3-169">Select **MVC Controller with views, using Entity Framework** and click **Ok**.</span></span>
* <span data-ttu-id="9c5a3-170">Définissez les paramètres **Classe de modèle** sur **Blog** et **Classe de contexte de données** sur **BloggingContext**.</span><span class="sxs-lookup"><span data-stu-id="9c5a3-170">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**.</span></span>
* <span data-ttu-id="9c5a3-171">Cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="9c5a3-171">Click **Add**.</span></span>


## <a name="run-the-application"></a><span data-ttu-id="9c5a3-172">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="9c5a3-172">Run the application</span></span>

<span data-ttu-id="9c5a3-173">Appuyez sur la touche F5 pour exécuter et tester l’application.</span><span class="sxs-lookup"><span data-stu-id="9c5a3-173">Press F5 to run and test the app.</span></span>

* <span data-ttu-id="9c5a3-174">Accédez à `/Blogs`.</span><span class="sxs-lookup"><span data-stu-id="9c5a3-174">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="9c5a3-175">Utilisez le lien de création pour créer des entrées de blog.</span><span class="sxs-lookup"><span data-stu-id="9c5a3-175">Use the create link to create some blog entries.</span></span> <span data-ttu-id="9c5a3-176">Testez les liens de détails et de suppression.</span><span class="sxs-lookup"><span data-stu-id="9c5a3-176">Test the details and delete links.</span></span>

![image](_static/create.png)

![image](_static/index-new-db.png)

## <a name="additional-resources"></a><span data-ttu-id="9c5a3-179">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="9c5a3-179">Additional Resources</span></span>

* <span data-ttu-id="9c5a3-180">[EF - Nouvelle base de données avec SQLite](xref:core/get-started/netcore/new-db-sqlite) : didacticiel sur la console multiplateforme EF</span><span class="sxs-lookup"><span data-stu-id="9c5a3-180">[EF - New database with SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
* [<span data-ttu-id="9c5a3-181">Introduction à ASP.NET Core MVC sur Mac ou Linux</span><span class="sxs-lookup"><span data-stu-id="9c5a3-181">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="9c5a3-182">Introduction à ASP.NET Core MVC avec Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9c5a3-182">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="9c5a3-183">Bien démarrer avec ASP.NET Core et Entity Framework Core à l’aide de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9c5a3-183">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)