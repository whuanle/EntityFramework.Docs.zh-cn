---
title: 值的转换的 EF 核心
author: ajcvickers
ms.author: divega
ms.date: 02/19/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/value-conversions
ms.openlocfilehash: 3e97c05a87ad9b4817c03f446031ea6c74704f5b
ms.sourcegitcommit: 605e42232854ce44bae09624a6eebc35b8e2473b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/16/2018
---
# <a name="value-conversions"></a>值的转换

> [!NOTE]  
> 此功能是在 EF 核心 2.1 的新增功能。

值转换器允许读取或写入数据库时要转换的属性值。 此转换可以是从某个值与另一个相同类型 （例如，加密字符串） 或从一种类型的值的另一种类型 （例如，转换枚举值到和从数据库中的字符串。） 的值

## <a name="fundamentals"></a>基础知识

值转换器指定方面`ModelClrType`和`ProviderClrType`。 模型类型是属性的中的实体类型的.NET 类型。 提供程序类型是理解数据库提供程序的.NET 类型。 例如，若要在数据库中的字符串的形式保存枚举，模型类型是一种枚举，和提供程序类型是`String`。 这两种类型可以是相同的。

使用两个定义转换`Func`表达式树： 一个从`ModelClrType`到`ProviderClrType`和从其他`ProviderClrType`到`ModelClrType`。 使用表达式树，这样它们可以编译到数据库访问代码的高效转换。 对于复杂的转换，表达式树可能简单调用执行转换的方法。

## <a name="configuring-a-value-converter"></a>配置值转换器

中的你 DbContext OnModelCreating 属性定义值的转换。 例如，考虑定义为枚举和实体类型：
```Csharp
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
然后可以在 OnModelCreating （例如存储为字符串的枚举值定义转换"驴女子"，"帮凶"，...）在数据库中：
```Csharp
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
> A`null`永远不会将该值传递到值转换器。 这使转换的实现更容易，并允许他们需要可以为 null 和不可为 null 属性之间共享。

## <a name="the-valueconverter-class"></a>ValueConverter 类

调用`HasConversion`如上所示将创建`ValueConverter`实例并将其设置的属性上。 `ValueConverter`改为显式创建。 例如：
```Csharp
var converter = new ValueConverter<EquineBeast, string>(
    v => v.ToString(),
    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
当多个属性使用相同的转换时，这很有用。

> [!NOTE]  
> 目前没有办法在一个位置中指定给定类型的每个属性，必须使用相同的值转换器。 此功能将考虑对将来的版本。

## <a name="built-in-converters"></a>内置的转换器

EF 核心附带了一组预定义`ValueConverter`类，在中找到`Microsoft.EntityFrameworkCore.Storage.ValueConversion`命名空间。 这些是：
* `BoolToZeroOneConverter` -Bool 到零，另一个
* `BoolToStringConverter` -Bool 为字符串，如"Y"和"N"
* `BoolToTwoValuesConverter` -Bool 到任何两个值
* `BytesToStringConverter` 为 Base64 编码的字符串的字节数组
* `CastingConverter` -转换需要仅 Csharp 强制转换
* `CharToStringConverter` -Char 为单个字符的字符串
* `DateTimeOffsetToBinaryConverter` 的为二进制编码的 64 位值 DateTimeOffset
* `DateTimeOffsetToBytesConverter` 的指向字节数组 DateTimeOffset
* `DateTimeOffsetToStringConverter` -为字符串 DateTimeOffset
* `DateTimeToBinaryConverter` -为 64 位值包括 DateTimeKind DateTime
* `DateTimeToStringConverter` -从 DateTime 到 string
* `DateTimeToTicksConverter` 的为刻度 DateTime
* `EnumToNumberConverter` 的枚举基础数字
* `EnumToStringConverter` -为字符串枚举
* `GuidToBytesConverter` 的指向字节数组 Guid
* `GuidToStringConverter` 的为字符串 Guid
* `NumberToBytesConverter` -指向字节数组任何数字值
* `NumberToStringConverter` -为字符串任何数字值
* `StringToBytesConverter` -为 UTF8 字节字符串
* `TimeSpanToStringConverter` -时间跨度为字符串
* `TimeSpanToTicksConverter` 的为刻度时间跨度

请注意，`EnumToStringConverter`包含在此列表。 这意味着没有无需显式，如上所示指定转换。 相反，只需使用内置的转换器：
```Csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
注意，所有内置转换器是无状态，因此单个实例可以安全地共享由多个属性。

## <a name="pre-defined-conversions"></a>预定义的转换

对于常见转换为其存在内置转换器没有无需显式指定转换器。 相反，只需配置应使用哪种提供程序类型和 EF 将自动使用适当的生成转换器。 枚举字符串的转换用作上例中，但 EF 将实际执行此操作自动如果配置的提供程序类型：
```Csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```
可以通过显式指定列类型来实现相同的操作。 例如，如果实体类型的定义，如以便：
```Csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```
然后枚举值将另存为无需在 OnModelCreating 任何进一步配置数据库中的字符串。

## <a name="limitations"></a>限制

有几个已知的当前限制的值转换系统：
* 如上所述，`null`无法转换。
* 目前没有办法分布到多个列或进行相反的转换的转换的一个属性。
* 使用的值转换可能会影响 EF 核心能够把表达式转换到 SQL。 为这种情况下，将记录警告。
正在为未来版本考虑删除这些限制。
