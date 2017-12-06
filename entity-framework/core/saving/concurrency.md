---
title: "处理并发的 EF 核心"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bce0539d-b0cd-457d-be71-f7ca16f3baea
ms.technology: entity-framework-core
uid: core/saving/concurrency
ms.openlocfilehash: bbd3e154c1b27b16c7d8f8fbf9ed51df0849795c
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
---
# <a name="handling-concurrency"></a><span data-ttu-id="2dae2-102">处理并发</span><span class="sxs-lookup"><span data-stu-id="2dae2-102">Handling Concurrency</span></span>

<span data-ttu-id="2dae2-103">如果属性配置为并发标记 EF 将检查与该记录中保存更改时，没有其他用户已修改数据库中的该值。</span><span class="sxs-lookup"><span data-stu-id="2dae2-103">If a property is configured as a concurrency token then EF will check that no other user has modified that value in the database when saving changes to that record.</span></span>

> [!TIP]  
> <span data-ttu-id="2dae2-104">你可以查看这篇文章[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/)GitHub 上。</span><span class="sxs-lookup"><span data-stu-id="2dae2-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/) on GitHub.</span></span>

## <a name="how-concurrency-handling-works-in-ef-core"></a><span data-ttu-id="2dae2-105">在 EF 核心中的并发处理工作原理</span><span class="sxs-lookup"><span data-stu-id="2dae2-105">How concurrency handling works in EF Core</span></span>

<span data-ttu-id="2dae2-106">在实体框架核心中并发处理的工作原理的详细说明，请参阅[并发标记](../modeling/concurrency.md)。</span><span class="sxs-lookup"><span data-stu-id="2dae2-106">For a detailed description of how concurrency handling works in Entity Framework Core, see [Concurrency Tokens](../modeling/concurrency.md).</span></span>

## <a name="resolving-concurrency-conflicts"></a><span data-ttu-id="2dae2-107">解决并发冲突</span><span class="sxs-lookup"><span data-stu-id="2dae2-107">Resolving concurrency conflicts</span></span>

<span data-ttu-id="2dae2-108">解决并发冲突的方法包括使用算法挂起的更改，可从当前用户与数据库中所做的更改合并。</span><span class="sxs-lookup"><span data-stu-id="2dae2-108">Resolving a concurrency conflict involves using an algorithm to merge the pending changes from the current user with the changes made in the database.</span></span> <span data-ttu-id="2dae2-109">确切的方法取决于你的应用程序，但常见方法是向用户显示的值并将它们决定要存储在数据库的正确值。</span><span class="sxs-lookup"><span data-stu-id="2dae2-109">The exact approach will vary based on your application, but a common approach is to display the values to the user and have them decide the correct values to be stored in the database.</span></span>

<span data-ttu-id="2dae2-110">**有三个组的值可用于帮助解决并发冲突。**</span><span class="sxs-lookup"><span data-stu-id="2dae2-110">**There are three sets of values available to help resolve a concurrency conflict.**</span></span>

* <span data-ttu-id="2dae2-111">**当前值**是应用程序尝试写入数据库的值。</span><span class="sxs-lookup"><span data-stu-id="2dae2-111">**Current values** are the values that the application was attempting to write to the database.</span></span>

* <span data-ttu-id="2dae2-112">**原始值**是最初检索到从数据库中执行的任何编辑之前的值。</span><span class="sxs-lookup"><span data-stu-id="2dae2-112">**Original values** are the values that were originally retrieved from the database, before any edits were made.</span></span>

* <span data-ttu-id="2dae2-113">**数据库值**是当前数据库中存储的值。</span><span class="sxs-lookup"><span data-stu-id="2dae2-113">**Database values** are the values currently stored in the database.</span></span>

<span data-ttu-id="2dae2-114">若要处理并发冲突，捕获`DbUpdateConcurrencyException`期间`SaveChanges()`，使用`DbUpdateConcurrencyException.Entries`对于受影响的实体，准备一组新的更改，然后重试`SaveChanges()`操作。</span><span class="sxs-lookup"><span data-stu-id="2dae2-114">To handle a concurrency conflict, catch a `DbUpdateConcurrencyException` during `SaveChanges()`, use `DbUpdateConcurrencyException.Entries` to prepare a new set of changes for the affected entities, and then retry the `SaveChanges()` operation.</span></span>

<span data-ttu-id="2dae2-115">在下面的示例中，`Person.FirstName`和`Person.LastName`是并发标记作为安装程序。</span><span class="sxs-lookup"><span data-stu-id="2dae2-115">In the following example, `Person.FirstName` and `Person.LastName` are setup as concurrency token.</span></span> <span data-ttu-id="2dae2-116">没有`// TODO:`其中应包括应用程序特定逻辑，以选择要保存到数据库的值的位置中的注释。</span><span class="sxs-lookup"><span data-stu-id="2dae2-116">There is a `// TODO:` comment in the location where you would include application specific logic to choose the value to be saved to the database.</span></span>

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Concurrency/Sample.cs?highlight=53,54)] -->
``` csharp
using Microsoft.EntityFrameworkCore;
using System;
using System.ComponentModel.DataAnnotations;
using System.Linq;

namespace EFSaving.Concurrency
{
    public class Sample
    {
        public static void Run()
        {
            // Ensure database is created and has a person in it
            using (var context = new PersonContext())
            {
                context.Database.EnsureDeleted();
                context.Database.EnsureCreated();

                context.People.Add(new Person { FirstName = "John", LastName = "Doe" });
                context.SaveChanges();
            }

            using (var context = new PersonContext())
            {
                // Fetch a person from database and change phone number
                var person = context.People.Single(p => p.PersonId == 1);
                person.PhoneNumber = "555-555-5555";

                // Change the persons name in the database (will cause a concurrency conflict)
                context.Database.ExecuteSqlCommand("UPDATE dbo.People SET FirstName = 'Jane' WHERE PersonId = 1");

                try
                {
                    // Attempt to save changes to the database
                    context.SaveChanges();
                }
                catch (DbUpdateConcurrencyException ex)
                {
                    foreach (var entry in ex.Entries)
                    {
                        if (entry.Entity is Person)
                        {
                            // Using a NoTracking query means we get the entity but it is not tracked by the context
                            // and will not be merged with existing entities in the context.
                            var databaseEntity = context.People.AsNoTracking().Single(p => p.PersonId == ((Person)entry.Entity).PersonId);
                            var databaseEntry = context.Entry(databaseEntity);

                            foreach (var property in entry.Metadata.GetProperties())
                            {
                                var proposedValue = entry.Property(property.Name).CurrentValue;
                                var originalValue = entry.Property(property.Name).OriginalValue;
                                var databaseValue = databaseEntry.Property(property.Name).CurrentValue;

                                // TODO: Logic to decide which value should be written to database
                                // entry.Property(property.Name).CurrentValue = <value to be saved>;

                                // Update original values to
                                entry.Property(property.Name).OriginalValue = databaseEntry.Property(property.Name).CurrentValue;
                            }
                        }
                        else
                        {
                            throw new NotSupportedException("Don't know how to handle concurrency conflicts for " + entry.Metadata.Name);
                        }
                    }

                    // Retry the save operation
                    context.SaveChanges();
                }
            }
        }

        public class PersonContext : DbContext
        {
            public DbSet<Person> People { get; set; }

            protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
            {
                optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFSaving.Concurrency;Trusted_Connection=True;");
            }
        }

        public class Person
        {
            public int PersonId { get; set; }

            [ConcurrencyCheck]
            public string FirstName { get; set; }

            [ConcurrencyCheck]
            public string LastName { get; set; }

            public string PhoneNumber { get; set; }
        }

    }
}
```
