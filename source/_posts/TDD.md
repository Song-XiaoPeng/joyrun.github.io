---
title: "单元测试进阶：理解测试驱动开发（TDD）"
date: 2016-08-14 21:41:32
author:     "Wiki"
header-img: "post-bg-2015.jpg"
tags:
    - Android
---

通常我们设计一个应用程序，并且快速创建单元测试来验证我们的设计，在我们编写这些测试时，这些测试也可以帮助改善了我最初的设计。随着我们编写越来越多的单元测试，正反馈的良性循环也会鼓励我们尽早地编写单元测试。当我们设计并实现时，就自然地想要知道我们将会如何来测试一个类。基于这一方法论，越来越多的开发者正从利于测试跃迁到测试驱动开发。

#### 通过简单例子来理解TDD

下面我们就从"保留两位小数,不能四舍五入"的例子理解什么是测试驱动开发。

##### 问题：

编写一个方法，输入double参数，返回保留两位小数，不能四舍五入的String。

##### 第一步：编写测试用例

按照我们预期的结果，编写参数化的单元测试用例

```java
@RunWith(value = Parameterized.class)
public class keep2DecimalParameterizedTest {
    private String expected;
    private double param0;
    @Parameterized.Parameters
    public static Collection<Object[]> getTestParameters() {
        return Arrays.asList(new Object[][]{
                {"1.00", 1.002},
                {"2.15", 2.156},
                {"3.14", 3.141}
        });
    }
    public keep2DecimalParameterizedTest(String expected, double param0) {
        this.expected = expected;
        this.param0 = param0;
    }
    @Test
    public void add() throws Exception {
        Assert.assertEquals(expected, Compute.keep2Decimal(param0));
    }
}
```

我们现在运行这个测试用例肯定是失败的，因为Compute.keep2Decimal()这个方法我们还没有实现，下面我们就要创建这个类和方法，并实现相关逻辑。

##### 第二步：编写实现

根据测试用例实现该方法的功能

```java
public class Compute {
    /*保留两位小数,不能四舍五入*/
    public static String keep2Decimal(double arg0) {
        return String.format("%.2f", arg0);
    }
}
```

##### 第三步：运行测试

运行单元测试，发现参数化测试中的第二项失败了Compute.keep2Decimal(2.156)，运行的结果是`2.16`,但是我们预期的结果是`2.15`，

##### 第四步：改进实现

```java
public class Compute {
    /*保留两位小数,不能四舍五入*/
    public static String keep2Decimal(double arg0) {
        return String.format("%.2f", (int) (arg0 * 100) /100.0);
    }
}
```

##### 第五步：重新测试

测试通过

### 上述例子分析

传统的开发周期是由以下环节组成的：编码、测试、(重复)、提交。开发者使用TDD后则做出了一个似乎很微小但是实际上却惊人有效的调整：测试、编码、(重复)、提交。测试推动了设计，并成为了方法的第一个客户。

