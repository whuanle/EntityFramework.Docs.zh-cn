---
title: 值的转换的 EF Core
author: ajcvickers
ms.date: 02/19/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/value-conversions
ms.openlocfilehash: 2a1956221ecc920feba796e4d95cc97259e89c53
ms.sourcegitcommit: 0cef7d448e1e47bdb333002e2254ed42d57b45b6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/29/2018
ms.locfileid: "43152505"
---
# <a name="value-conversions"></a><span data-ttu-id="934c6-102">值的转换</span><span class="sxs-lookup"><span data-stu-id="934c6-102">Value Conversions</span></span>

> [!NOTE]  
> <span data-ttu-id="934c6-103">此功能是在 EF Core 2.1 的新增功能。</span><span class="sxs-lookup"><span data-stu-id="934c6-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="934c6-104">值转换器允许读取或写入数据库时要转换的属性值。</span><span class="sxs-lookup"><span data-stu-id="934c6-104">Value converters allow property values to be converted when reading from or writing to the database.</span></span> <span data-ttu-id="934c6-105">此转换可以是从某个值与另一个相同类型 （例如，加密字符串） 或值为一种类型的值的另一种类型 （例如，转换的枚举值和从数据库中的字符串。）</span><span class="sxs-lookup"><span data-stu-id="934c6-105">This conversion can be from one value to another of the same type (for example, encrypting strings) or from a value of one type to a value of another type (for example, converting enum values to and from strings in the database.)</span></span>

## <a name="fundamentals"></a><span data-ttu-id="934c6-106">基础知识</span><span class="sxs-lookup"><span data-stu-id="934c6-106">Fundamentals</span></span>

<span data-ttu-id="934c6-107">指定的值转换器`ModelClrType`和一个`ProviderClrType`。</span><span class="sxs-lookup"><span data-stu-id="934c6-107">Value converters are specified in terms of a `ModelClrType` and a `ProviderClrType`.</span></span> <span data-ttu-id="934c6-108">模型类型是属性的实体类型中的.NET 类型。</span><span class="sxs-lookup"><span data-stu-id="934c6-108">The model type is the .NET type of the property in the entity type.</span></span> <span data-ttu-id="934c6-109">提供程序类型是理解的数据库提供程序的.NET 类型。</span><span class="sxs-lookup"><span data-stu-id="934c6-109">The provider type is the .NET type understood by the database provider.</span></span> <span data-ttu-id="934c6-110">例如，若要保存为数据库中的字符串的枚举，模型类型是枚举的类型和提供程序类型是`String`。</span><span class="sxs-lookup"><span data-stu-id="934c6-110">For example, to save enums as strings in the database, the model type is the type of the enum, and the provider type is `String`.</span></span> <span data-ttu-id="934c6-111">这两种类型可以是相同的。</span><span class="sxs-lookup"><span data-stu-id="934c6-111">These two types can be the same.</span></span>

<span data-ttu-id="934c6-112">使用两个定义的转换`Func`表达式树： 一个来自`ModelClrType`到`ProviderClrType`以及从其他`ProviderClrType`到`ModelClrType`。</span><span class="sxs-lookup"><span data-stu-id="934c6-112">Conversions are defined using two `Func` expression trees: one from `ModelClrType` to `ProviderClrType` and the other from `ProviderClrType` to `ModelClrType`.</span></span> <span data-ttu-id="934c6-113">使用表达式树，这样它们可以编译到数据库访问代码的有效转换。</span><span class="sxs-lookup"><span data-stu-id="934c6-113">Expression trees are used so that they can be compiled into the database access code for efficient conversions.</span></span> <span data-ttu-id="934c6-114">对于复杂的转换，表达式树可能是对执行转换的方法的简单调用。</span><span class="sxs-lookup"><span data-stu-id="934c6-114">For complex conversions, the expression tree may be a simple call to a method that performs the conversion.</span></span>

## <a name="configuring-a-value-converter"></a><span data-ttu-id="934c6-115">配置值转换器</span><span class="sxs-lookup"><span data-stu-id="934c6-115">Configuring a value converter</span></span>

<span data-ttu-id="934c6-116">中的属性定义值的转换`OnModelCreating`的你`DbContext`。</span><span class="sxs-lookup"><span data-stu-id="934c6-116">Value conversions are defined on properties in the `OnModelCreating` of your `DbContext`.</span></span> <span data-ttu-id="934c6-117">有关示例，请定义为枚举和实体类型：</span><span class="sxs-lookup"><span data-stu-id="934c6-117">For example, consider an enum and entity type defined as:</span></span>
``` csharp
public class Rider
{
    public int Id { get; set; }
    public EquineBeast Mount { get; set; }
}

public enum EquineBeast
{
    Donkey,
    Mule,
    Horse,
    Unicorn
}
```
<span data-ttu-id="934c6-118">然后可以在中定义的转换`OnModelCreating`存储为字符串 （例如，"驴女子"、"帮凶"，...） 在数据库中的枚举值：</span><span class="sxs-lookup"><span data-stu-id="934c6-118">Then conversions can be defined in `OnModelCreating` to store the enum values as strings (for example, "Donkey", "Mule", ...) in the database:</span></span>
``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder
        .Entity<Rider>()
        .Property(e => e.Mount)
        .HasConversion(
            v => v.ToString(),
            v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));
}
```
> [!NOTE]  
> <span data-ttu-id="934c6-119">一个`null`永远不会将该值传递到值转换器。</span><span class="sxs-lookup"><span data-stu-id="934c6-119">A `null` value will never be passed to a value converter.</span></span> <span data-ttu-id="934c6-120">这使转换的实现更轻松，并允许他们在可以为 null 和不可为 null 属性之间共享。</span><span class="sxs-lookup"><span data-stu-id="934c6-120">This makes the implementation of conversions easier and allows them to be shared amongst nullable and non-nullable properties.</span></span>

## <a name="the-valueconverter-class"></a><span data-ttu-id="934c6-121">ValueConverter 类</span><span class="sxs-lookup"><span data-stu-id="934c6-121">The ValueConverter class</span></span>

<span data-ttu-id="934c6-122">调用`HasConversion`如上所示将创建`ValueConverter`实例，并将其设置的属性。</span><span class="sxs-lookup"><span data-stu-id="934c6-122">Calling `HasConversion` as shown above will create a `ValueConverter` instance and set it on the property.</span></span> <span data-ttu-id="934c6-123">`ValueConverter`改为显式创建。</span><span class="sxs-lookup"><span data-stu-id="934c6-123">The `ValueConverter` can instead be created explicitly.</span></span> <span data-ttu-id="934c6-124">例如：</span><span class="sxs-lookup"><span data-stu-id="934c6-124">For example:</span></span>
``` csharp
var converter = new ValueConverter<EquineBeast, string>(
    v => v.ToString(),
    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
<span data-ttu-id="934c6-125">当多个属性使用相同的转换时，这很有用。</span><span class="sxs-lookup"><span data-stu-id="934c6-125">This can be useful when multiple properties use the same conversion.</span></span>

> [!NOTE]  
> <span data-ttu-id="934c6-126">目前没有办法在一个位置指定给定类型的每个属性必须使用相同的值转换器。</span><span class="sxs-lookup"><span data-stu-id="934c6-126">There is currently no way to specify in one place that every property of a given type must use the same value converter.</span></span> <span data-ttu-id="934c6-127">此功能将被视为在将来的版本。</span><span class="sxs-lookup"><span data-stu-id="934c6-127">This feature will be considered for a future release.</span></span>

## <a name="built-in-converters"></a><span data-ttu-id="934c6-128">内置的转换器</span><span class="sxs-lookup"><span data-stu-id="934c6-128">Built-in converters</span></span>

<span data-ttu-id="934c6-129">EF Core附带了一组预定义`ValueConverter`类，在中找到`Microsoft.EntityFrameworkCore.Storage.ValueConversion`命名空间。</span><span class="sxs-lookup"><span data-stu-id="934c6-129">EF Core ships with a set of pre-defined `ValueConverter` classes, found in the `Microsoft.EntityFrameworkCore.Storage.ValueConversion` namespace.</span></span> <span data-ttu-id="934c6-130">这些是：</span><span class="sxs-lookup"><span data-stu-id="934c6-130">These are:</span></span>
* <span data-ttu-id="934c6-131">`BoolToZeroOneConverter` -Bool 零和一到</span><span class="sxs-lookup"><span data-stu-id="934c6-131">`BoolToZeroOneConverter` - Bool to zero and one</span></span>
* <span data-ttu-id="934c6-132">`BoolToStringConverter` -布尔值为"Y"和"N"等字符串</span><span class="sxs-lookup"><span data-stu-id="934c6-132">`BoolToStringConverter` - Bool to strings such as "Y" and "N"</span></span>
* <span data-ttu-id="934c6-133">`BoolToTwoValuesConverter` -Bool 到任意两个值</span><span class="sxs-lookup"><span data-stu-id="934c6-133">`BoolToTwoValuesConverter` - Bool to any two values</span></span>
* <span data-ttu-id="934c6-134">`BytesToStringConverter` 为 Base64 编码的字符串的字节数组</span><span class="sxs-lookup"><span data-stu-id="934c6-134">`BytesToStringConverter` - Byte array to Base64-encoded string</span></span>
* <span data-ttu-id="934c6-135">`CastingConverter` -转换需要类型转换</span><span class="sxs-lookup"><span data-stu-id="934c6-135">`CastingConverter` - Conversions that require only a type cast</span></span>
* <span data-ttu-id="934c6-136">`CharToStringConverter` -Char 到单字符字符串</span><span class="sxs-lookup"><span data-stu-id="934c6-136">`CharToStringConverter` - Char to single character string</span></span>
* <span data-ttu-id="934c6-137">`DateTimeOffsetToBinaryConverter` 的为二进制编码的 64 位值 DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="934c6-137">`DateTimeOffsetToBinaryConverter` - DateTimeOffset to binary-encoded 64-bit value</span></span>
* <span data-ttu-id="934c6-138">`DateTimeOffsetToBytesConverter` -DateTimeOffset 到字节数组</span><span class="sxs-lookup"><span data-stu-id="934c6-138">`DateTimeOffsetToBytesConverter` - DateTimeOffset to byte array</span></span>
* <span data-ttu-id="934c6-139">`DateTimeOffsetToStringConverter` 的为字符串 DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="934c6-139">`DateTimeOffsetToStringConverter` - DateTimeOffset to string</span></span>
* <span data-ttu-id="934c6-140">`DateTimeToBinaryConverter` 的为包括 DateTimeKind 的 64 位值日期时间</span><span class="sxs-lookup"><span data-stu-id="934c6-140">`DateTimeToBinaryConverter` - DateTime to 64-bit value including DateTimeKind</span></span>
* <span data-ttu-id="934c6-141">`DateTimeToStringConverter` -从 DateTime 到 string</span><span class="sxs-lookup"><span data-stu-id="934c6-141">`DateTimeToStringConverter` - DateTime to string</span></span>
* <span data-ttu-id="934c6-142">`DateTimeToTicksConverter` 的为刻度日期时间</span><span class="sxs-lookup"><span data-stu-id="934c6-142">`DateTimeToTicksConverter` - DateTime to ticks</span></span>
* <span data-ttu-id="934c6-143">`EnumToNumberConverter` 的枚举基础数字</span><span class="sxs-lookup"><span data-stu-id="934c6-143">`EnumToNumberConverter` - Enum to underlying number</span></span>
* <span data-ttu-id="934c6-144">`EnumToStringConverter` 的为字符串枚举</span><span class="sxs-lookup"><span data-stu-id="934c6-144">`EnumToStringConverter` - Enum to string</span></span>
* <span data-ttu-id="934c6-145">`GuidToBytesConverter` -Guid 传递给字节数组</span><span class="sxs-lookup"><span data-stu-id="934c6-145">`GuidToBytesConverter` - Guid to byte array</span></span>
* <span data-ttu-id="934c6-146">`GuidToStringConverter` -Guid 传递给字符串</span><span class="sxs-lookup"><span data-stu-id="934c6-146">`GuidToStringConverter` - Guid to string</span></span>
* <span data-ttu-id="934c6-147">`NumberToBytesConverter` 的指向字节数组任意数字值</span><span class="sxs-lookup"><span data-stu-id="934c6-147">`NumberToBytesConverter` - Any numerical value to byte array</span></span>
* <span data-ttu-id="934c6-148">`NumberToStringConverter` 的为字符串任意数字值</span><span class="sxs-lookup"><span data-stu-id="934c6-148">`NumberToStringConverter` - Any numerical value to string</span></span>
* <span data-ttu-id="934c6-149">`StringToBytesConverter` -为 UTF8 字节字符串</span><span class="sxs-lookup"><span data-stu-id="934c6-149">`StringToBytesConverter` - String to UTF8 bytes</span></span>
* <span data-ttu-id="934c6-150">`TimeSpanToStringConverter` 的为字符串时间跨度</span><span class="sxs-lookup"><span data-stu-id="934c6-150">`TimeSpanToStringConverter` - TimeSpan to string</span></span>
* <span data-ttu-id="934c6-151">`TimeSpanToTicksConverter` 的为刻度时间跨度</span><span class="sxs-lookup"><span data-stu-id="934c6-151">`TimeSpanToTicksConverter` - TimeSpan to ticks</span></span>

<span data-ttu-id="934c6-152">请注意，`EnumToStringConverter`包含在此列表。</span><span class="sxs-lookup"><span data-stu-id="934c6-152">Notice that `EnumToStringConverter` is included in this list.</span></span> <span data-ttu-id="934c6-153">这意味着，，无需转换显式指定，如上所示。</span><span class="sxs-lookup"><span data-stu-id="934c6-153">This means that there is no need to specify the conversion explicitly, as shown above.</span></span> <span data-ttu-id="934c6-154">相反，只需使用内置的转换器：</span><span class="sxs-lookup"><span data-stu-id="934c6-154">Instead, just use the built-in converter:</span></span>
``` csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
<span data-ttu-id="934c6-155">请注意，所有内置的转换器是无状态，因此单个实例可以安全地共享多个属性。</span><span class="sxs-lookup"><span data-stu-id="934c6-155">Note that all the built-in converters are stateless and so a single instance can be safely shared by multiple properties.</span></span>

## <a name="pre-defined-conversions"></a><span data-ttu-id="934c6-156">预定义的转换</span><span class="sxs-lookup"><span data-stu-id="934c6-156">Pre-defined conversions</span></span>

<span data-ttu-id="934c6-157">对于存在内置的转换器的常见转换是无需显式指定转换器。</span><span class="sxs-lookup"><span data-stu-id="934c6-157">For common conversions for which a built-in converter exists there is no need to specify the converter explicitly.</span></span> <span data-ttu-id="934c6-158">相反，只需配置应使用哪种提供程序类型和 EF 将自动使用适当的内置转换器。</span><span class="sxs-lookup"><span data-stu-id="934c6-158">Instead, just configure which provider type should be used and EF will automatically use the appropriate built-in converter.</span></span> <span data-ttu-id="934c6-159">更高版本，例如使用到字符串的转换的枚举，但 EF 将实际执行此操作自动如果配置的提供程序类型：</span><span class="sxs-lookup"><span data-stu-id="934c6-159">Enum to string conversions are used as an example above, but EF will actually do this automatically if the provider type is configured:</span></span>
``` csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```
<span data-ttu-id="934c6-160">可以通过显式指定列类型来实现相同的操作。</span><span class="sxs-lookup"><span data-stu-id="934c6-160">The same thing can be achieved by explicitly specifying the column type.</span></span> <span data-ttu-id="934c6-161">例如，如果实体类型的定义类似于这样：</span><span class="sxs-lookup"><span data-stu-id="934c6-161">For example, if the entity type is defined like so:</span></span>
``` csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```
<span data-ttu-id="934c6-162">枚举值将保存为没有任何进一步的配置，在数据库中的字符串`OnModelCreating`。</span><span class="sxs-lookup"><span data-stu-id="934c6-162">Then the enum values will be saved as strings in the database without any further configuration in `OnModelCreating`.</span></span>

## <a name="limitations"></a><span data-ttu-id="934c6-163">限制</span><span class="sxs-lookup"><span data-stu-id="934c6-163">Limitations</span></span>

<span data-ttu-id="934c6-164">有几个已知的当前限制的值转换系统：</span><span class="sxs-lookup"><span data-stu-id="934c6-164">There are a few known current limitations of the value conversion system:</span></span>
* <span data-ttu-id="934c6-165">如上所述，`null`无法转换。</span><span class="sxs-lookup"><span data-stu-id="934c6-165">As noted above, `null` cannot be converted.</span></span>
* <span data-ttu-id="934c6-166">目前没有办法分散到多个列或进行相反转换的转换的一个属性。</span><span class="sxs-lookup"><span data-stu-id="934c6-166">There is currently no way to spread a conversion of one property to multiple columns or vice-versa.</span></span>
* <span data-ttu-id="934c6-167">使用的值转换可能会影响 EF Core能够把表达式转换到 SQL。</span><span class="sxs-lookup"><span data-stu-id="934c6-167">Use of value conversions may impact the ability of EF Core to translate expressions to SQL.</span></span> <span data-ttu-id="934c6-168">对于这种情况下，将记录警告。</span><span class="sxs-lookup"><span data-stu-id="934c6-168">A warning will be logged for such cases.</span></span>
<span data-ttu-id="934c6-169">删除这些限制正在考虑针对将来的版本。</span><span class="sxs-lookup"><span data-stu-id="934c6-169">Removal of these limitations is being considered for a future release.</span></span>
