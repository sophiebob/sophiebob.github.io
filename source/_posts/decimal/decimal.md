---
title: DecimalFormat四舍五入
date: 2018-03-19 15:14:09
tags: [java]
categories: java
keywords: DecimalFormat
---

`DecimalFormat` 是 `NumberFormat` 的一个具体子类，用于格式化十进制数字。
该类设计有各种功能，使其能够分析和格式化任意语言环境中的数，包括对西方语言、阿拉伯语和印度语数字的支持。它还支持不同类型的数，包括整数 (123)、定点数 (123.4)、科学记数法表示的数 (1.23E4)、百分数 (12%) 和金额 ($123)。所有这些内容都可以本地化。

#### 特殊模式字符
- `0`  阿拉伯数字
- `#`  阿拉伯数字，如果不存在则显示为空(不包括 0)
- `.`  小数分隔符或货币小数分隔符
- `-`  负数前缀
- `%`  乘以 100 和作为百分比显示 
- `?`  乘以 1000 和作为千进制货币符显示

#### 举例

```
System.out.println(new DecimalFormat("0.0").format(new Double("12.34")));
System.out.println(new DecimalFormat("#.#").format(new Double("12.34")));
System.out.println(new DecimalFormat("000.000").format(new Double("12.34")));
System.out.println(new DecimalFormat("###.###").format(new Double("12.34")));
System.out.println(new DecimalFormat("0%").format(new Double("12.34")));

结果============

12.3
12.3
012.340
12.34
1234%
```

#### 四舍五入
DecimalForma函数默认的四舍五入的方法是银行家算法。四舍六入五考虑，五后非零就进一，五后为零看奇偶，五前为偶应舍去，五前为奇要进一
```
System.out.println(new DecimalFormat("##.#").format(new Double("88.85")));
System.out.println(new DecimalFormat("##.#").format(new Double("88.851")));
结果============
88.8
88.9
```
注：此`四舍五入`和我们通常理解的不一样，绝对是大坑。`DecimalFormat` 提供了public DecimalFormat (String pattern, DecimalFormatSymbols symbols)构造方法，默认情况下，它使用 `RoundingMode.HALF_EVEN`。
RoundingMode是一个枚举类，有一下几个常量：`UP`，`DOWN`，`CEILING`，`FLOOR`，`HALF_UP`，`HALF_DOWN`，`HALF_EVEN`，`UNNECESSARY`。
