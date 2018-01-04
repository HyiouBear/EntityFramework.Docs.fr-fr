---
title: "Création d’un modèle - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
ms.technology: entity-framework-core
uid: core/modeling/index
ms.openlocfilehash: 1ad0f6891fbc8ba2e4d102cc9997f053a9dddb66
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2017
---
# <a name="creating-a-model"></a><span data-ttu-id="a764f-102">Création d’un modèle</span><span class="sxs-lookup"><span data-stu-id="a764f-102">Creating a Model</span></span>

<span data-ttu-id="a764f-103">Entity Framework utilise un ensemble de conventions pour créer un modèle basé sur la forme de vos classes d’entité.</span><span class="sxs-lookup"><span data-stu-id="a764f-103">Entity Framework uses a set of conventions to build a model based on the shape of your entity classes.</span></span> <span data-ttu-id="a764f-104">Vous pouvez spécifier une configuration supplémentaire pour compléter et/ou remplacer ce qui a été découvert par convention.</span><span class="sxs-lookup"><span data-stu-id="a764f-104">You can specify additional configuration to supplement and/or override what was discovered by convention.</span></span>

<span data-ttu-id="a764f-105">Cet article traite de la configuration qui peut être appliquée à un modèle ciblant n’importe quel magasin de données et qui peut être appliquée pendant le ciblage d’une base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="a764f-105">This article covers configuration that can be applied to a model targeting any data store and that which can be applied when targeting any relational database.</span></span> <span data-ttu-id="a764f-106">Les fournisseurs peuvent également activer la configuration qui est spécifique à un magasin de données particulier.</span><span class="sxs-lookup"><span data-stu-id="a764f-106">Providers may also enable configuration that is specific to a particular data store.</span></span> <span data-ttu-id="a764f-107">Pour plus d’informations sur la configuration spécifique du fournisseur, consultez la section [Fournisseurs de base de données](../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="a764f-107">For documentation on provider specific configuration see the [Database Providers](../providers/index.md) section.</span></span>

> [!TIP]  
> <span data-ttu-id="a764f-108">Vous pouvez afficher [l’exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) proposé dans cet article sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="a764f-108">You can view this article’s [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) on GitHub.</span></span>

## <a name="methods-of-configuration"></a><span data-ttu-id="a764f-109">Méthodes de configuration</span><span class="sxs-lookup"><span data-stu-id="a764f-109">Methods of configuration</span></span>

### <a name="fluent-api"></a><span data-ttu-id="a764f-110">API Fluent</span><span class="sxs-lookup"><span data-stu-id="a764f-110">Fluent API</span></span>

<span data-ttu-id="a764f-111">Vous pouvez substituer la méthode `OnModelCreating` dans le contexte dérivé et utiliser `ModelBuilder API` pour configurer votre modèle.</span><span class="sxs-lookup"><span data-stu-id="a764f-111">You can override the `OnModelCreating` method in your derived context and use the `ModelBuilder API` to configure your model.</span></span> <span data-ttu-id="a764f-112">Il s’agit de la méthode de configuration la plus puissante, qui permet de spécifier une configuration sans modifier les classes d’entité.</span><span class="sxs-lookup"><span data-stu-id="a764f-112">This is the most powerful method of configuration and allows configuration to be specified without modifying your entity classes.</span></span> <span data-ttu-id="a764f-113">Dotée du niveau de priorité le plus élevé, la configuration de l’API Fluent remplace les conventions et les annotations de données.</span><span class="sxs-lookup"><span data-stu-id="a764f-113">Fluent API configuration has the highest precedence and will override conventions and data annotations.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Required.cs?range=5-15&highlight=5-10)] -->

``` csharp
    class MyContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder.Entity<Blog>()
                .Property(b => b.Url)
                .IsRequired();
        }
    }
```

### <a name="data-annotations"></a><span data-ttu-id="a764f-114">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="a764f-114">Data Annotations</span></span>

<span data-ttu-id="a764f-115">Vous pouvez également appliquer des attributs (également appelés annotations de données) à vos classes et propriétés.</span><span class="sxs-lookup"><span data-stu-id="a764f-115">You can also apply attributes (known as Data Annotations) to your classes and properties.</span></span> <span data-ttu-id="a764f-116">Les annotations de données remplacent les conventions, mais elles sont remplacées par la configuration de l’API Fluent.</span><span class="sxs-lookup"><span data-stu-id="a764f-116">Data annotations will override conventions, but will be overwritten by Fluent API configuration.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?range=11-16&highlight=4)] -->
``` csharp
    public class Blog
    {
        public int BlogId { get; set; }
        [Required]
        public string Url { get; set; }
    }
```