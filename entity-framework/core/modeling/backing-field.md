---
title: 备份 EF Core 的字段-
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
ms.technology: entity-framework-core
uid: core/modeling/backing-field
ms.openlocfilehash: e95417b3368d09a64f9ec02ae40c38dc770c2e6b
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2017
ms.locfileid: "26053457"
---
# <a name="backing-fields"></a><span data-ttu-id="16364-102">支持字段</span><span class="sxs-lookup"><span data-stu-id="16364-102">Backing Fields</span></span>

> [!NOTE]  
> <span data-ttu-id="16364-103">此功能是 EF Core 1.1 中的新增功能。</span><span class="sxs-lookup"><span data-stu-id="16364-103">This feature is new in EF Core 1.1.</span></span>

<span data-ttu-id="16364-104">支持字段允许 EF 读取和/或写入一个字段，而不是一个属性。</span><span class="sxs-lookup"><span data-stu-id="16364-104">Backing fields allow EF to read and/or write to a field rather than a property.</span></span> <span data-ttu-id="16364-105">这一点可能很有用，当在类中的封装用于限制的使用和/或增强围绕访问数据的语义由应用程序代码，但值应进行读取和/或写入到数据库而不使用这些限制时 /增强功能。</span><span class="sxs-lookup"><span data-stu-id="16364-105">This can be useful when encapsulation in the class is being used to restrict the use of and/or enhance the semantics around access to the data by application code, but the value should be read from and/or written to the database without using those restrictions/enhancements.</span></span>

## <a name="conventions"></a><span data-ttu-id="16364-106">约定</span><span class="sxs-lookup"><span data-stu-id="16364-106">Conventions</span></span>

<span data-ttu-id="16364-107">按照约定，将作为备份为给定属性 （按优先顺序列出） 的字段发现以下字段。</span><span class="sxs-lookup"><span data-stu-id="16364-107">By convention, the following fields will be discovered as backing fields for a given property (listed in precedence order).</span></span> <span data-ttu-id="16364-108">字段只会发现的模型中包含的属性。</span><span class="sxs-lookup"><span data-stu-id="16364-108">Fields are only discovered for properties that are included in the model.</span></span> <span data-ttu-id="16364-109">在模型中包含属性的详细信息，请参阅[包括和排除属性](included-properties.md)。</span><span class="sxs-lookup"><span data-stu-id="16364-109">For more information on which properties are included in the model, see [Including & Excluding Properties](included-properties.md).</span></span>

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/BackingField.cs#Sample)]

<span data-ttu-id="16364-110">当配置的支持字段时，EF 将直接写入该字段具体化数据库 （而不是使用属性 setter） 中的实体实例时。</span><span class="sxs-lookup"><span data-stu-id="16364-110">When a backing field is configured, EF will write directly to that field when materializing entity instances from the database (rather than using the property setter).</span></span> <span data-ttu-id="16364-111">如果 EF 需要读取或写入在其他时间值，它将尽可能使用该属性。</span><span class="sxs-lookup"><span data-stu-id="16364-111">If EF needs to read or write the value at other times, it will use the property if possible.</span></span> <span data-ttu-id="16364-112">例如，如果 EF 需要更新属性的值，它将使用属性 setter，如果已定义。</span><span class="sxs-lookup"><span data-stu-id="16364-112">For example, if EF needs to update the value for a property, it will use the property setter if one is defined.</span></span> <span data-ttu-id="16364-113">如果属性是只读的然后它将写入到字段。</span><span class="sxs-lookup"><span data-stu-id="16364-113">If the property is read-only, then it will write to the field.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="16364-114">数据注释</span><span class="sxs-lookup"><span data-stu-id="16364-114">Data Annotations</span></span>

<span data-ttu-id="16364-115">支持字段时，无法使用数据注释进行配置。</span><span class="sxs-lookup"><span data-stu-id="16364-115">Backing fields cannot be configured with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="16364-116">Fluent API</span><span class="sxs-lookup"><span data-stu-id="16364-116">Fluent API</span></span>

<span data-ttu-id="16364-117">Fluent API 可用于配置属性的支持字段。</span><span class="sxs-lookup"><span data-stu-id="16364-117">You can use the Fluent API to configure a backing field for a property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a><span data-ttu-id="16364-118">控制当使用此字段</span><span class="sxs-lookup"><span data-stu-id="16364-118">Controlling when the field is used</span></span>

<span data-ttu-id="16364-119">你可以配置当 EF 使用的字段或属性。</span><span class="sxs-lookup"><span data-stu-id="16364-119">You can configure when EF uses the field or property.</span></span> <span data-ttu-id="16364-120">请参阅[PropertyAccessMode 枚举](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode)有关支持的选项。</span><span class="sxs-lookup"><span data-stu-id="16364-120">See the [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) for the supported options.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a><span data-ttu-id="16364-121">不带属性的字段</span><span class="sxs-lookup"><span data-stu-id="16364-121">Fields without a property</span></span>

<span data-ttu-id="16364-122">你还可以在实体类中，没有相应的 CLR 属性，但改为使用字段来存储实体中的数据在模型中创建概念的属性。</span><span class="sxs-lookup"><span data-stu-id="16364-122">You can also create a conceptual property in your model that does not have a corresponding CLR property in the entity class, but instead uses a field to store the data in the entity.</span></span> <span data-ttu-id="16364-123">这是不同于[隐藏属性](shadow-properties.md)、 数据更改跟踪器中的存储位置。</span><span class="sxs-lookup"><span data-stu-id="16364-123">This is different from [Shadow Properties](shadow-properties.md), where the data is stored in the change tracker.</span></span> <span data-ttu-id="16364-124">此值通常将在实体类使用方法来获取/设置值使用。</span><span class="sxs-lookup"><span data-stu-id="16364-124">This would typically be used if the entity class uses methods to get/set values.</span></span>

<span data-ttu-id="16364-125">可让 EF 中的字段的名称`Property(...)`API。</span><span class="sxs-lookup"><span data-stu-id="16364-125">You can give EF the name of the field in the `Property(...)` API.</span></span> <span data-ttu-id="16364-126">如果不没有具有给定名称的任何属性，将查找字段 EF。</span><span class="sxs-lookup"><span data-stu-id="16364-126">If there is no property with the given name, then EF will look for a field.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldNoProperty.cs#Sample)]

<span data-ttu-id="16364-127">你还可以选择为之外的字段名称的名称的属性。</span><span class="sxs-lookup"><span data-stu-id="16364-127">You can also choose to give the property a name, other than the field name.</span></span> <span data-ttu-id="16364-128">创建模型，然后使用此名称，最值得注意的是它将用于映射到数据库中的列名称。</span><span class="sxs-lookup"><span data-stu-id="16364-128">This name is then used when creating the model, most notably it will be used for the column name that is mapped to in the database.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldConceptualProperty.cs#Sample)]

<span data-ttu-id="16364-129">当实体类中有任何属性时，你可以使用`EF.Property(...)`LINQ 查询来指代从概念上讲是模型的一部分的属性中的方法。</span><span class="sxs-lookup"><span data-stu-id="16364-129">When there is no property in the entity class, you can use the `EF.Property(...)` method in a LINQ query to refer to the property that is conceptually part of the model.</span></span>

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "Url"));
```
