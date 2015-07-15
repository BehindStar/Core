# Castle 动态代理 - 介绍

Castle 动态代理是一个在运行时生成轻量级 .NET 代理的库。代理对象允许拦截对象成员的调用，在不修改该类的代码的情况下。

动态代理与 CLR 内置的代理实现不同，不需要被代理类继承 `MarshalByRefObject`。通过继承 `MashalByRefObject` 代理对象是极其侵入式的，因为这样就不允许类继承其他类，也不允许类的透明代理。此外 Castle 动态代理提供标准 CLR 代理以外的功能，比如允许你加入多个对象。for example it lets you mix in multiple objects.

## 需求

要使用 Castle 动态代理，你需要以下环境：

* 安装其中一个运行时：
  * .NET 3.5 sp1 或以上
  * Silverlight 4 或以上
* `Castle.Core.dll` （动态代理所在的程序集）

:information_source: **动态代理程序集：** 在以前的版本中 （直到 2.2），动态代理在它自己的程序集 `Castle.DynamicProxy.dll` 中。之后它被移到了 `Castle.Core.dll` 中，使用时不再需要其它程序集了。

## 代理

首先，动态代理是什么，为什么要关心这个？动态代理（缩写为 DP），和它的名字一样，是一个框架，帮助你实现代理对象设计模式。这是代理部分。动态部分，指的是代理类型的实际创建发生在运行时，你可以动态生成（compose）代理对象。

根据维基百科：

> 一个代理，最常见的形式，是一个充当其他事物的接口的类。其他事物可以是任何事物：网络连接、内存中的大对象、文件、或其他重要或不可复制的资源。

另一种看待代理的方式，是与黑客帝国（The Matrix）比较。

![](images/matrix.jpg)

Matrix as analogy for proxies (plus it's a cool picture)

我想这个星球上，没人没看过这部电影，且不知道这里剧情的细节。无论如何，矩阵中的人并不是真正的人（“这个勺子并不存在”，记得吗？）。它们是真正的人的代理……它们看起来像人，它们的行为像，但在同时，它们不是他们。另一个含义是，代理有不同的规则。代理可以是它们代理的对象，但是它们可以是更多（飞行，躲避子弹，……）。更重要的是，代理通常将行为委托到它代理的对象（有点像 - “如果你在矩阵中被杀死，你在现实中也死了”）。

来自程序员日常工作中，透明代理最好的例子是 WCF 代理。从使用它们的代码的角度来看，它们就是一些实现了接口的对象。可以通过该接口来使用它，就像其他对象一样，即时所使用的接口的实际实现在其它计算机上面。

这是代理的一个例子，向它的用户隐藏了实际对象的地点，这也是你使用代理的一种方式。

## 拦截管道

另一种方式，也是使用 DP 最多的方式，是给被代理对象添加行为。这就是 `IInterceptor` 接口的用途。你可以使用拦截器将行为注入代理。

![](images/proxy-pipeline.png)

动态代理工作方式示意图。

上图显示了动态代理是如何工作的。

* 蓝色方框是代理。某人调用了代理上的方法（黄色箭头）。在方法到达目标对象之前，它经过一个拦截器管道。
* 每个拦截器得到一个 `IInvocation` 对象（动态代理的另一个重要接口），该对象保存有当前请求的所有信息，比如被调用方法的 `MethodInfo`，参数和返回值，代理的引用，以及被代理对象，和一些其他信息。每个拦截器都有机会检查和修改这些值，在目标对象的实际方法调用之前。所以在这个阶段你可以记录关于传递给方法的参数的调试信息，或者验证这些参数。然后，拦截器必须调用 `invocation.Proceed()` ，将控制权沿着管道传递。一个拦截器最多调用一次 `Proceed`，否则会抛出异常。
* 在最后一个拦截器调用 `Proceed` 之后，被代理对象的实际方法被调用。然后调用将原路返回，沿着管道向上（绿色箭头），给每个拦截器机会以检查和采取行动，返回值，或抛出异常。
* 最后，代理返回 `invocation.ReturnValue` 保存的值作为被调用方法的返回值。

### 拦截器示例

如果还不明白，这里有一个示例拦截器，显示它是怎样工作的：

```csharp
[Serializable]
public class Interceptor : IInterceptor
{
    public void Intercept(IInvocation invocation)
    {
        Console.WriteLine("Before target call");
        try
        {
           invocation.Proceed();
        }
        catch(Exception)
        {
           Console.WriteLine("Target threw an exception!");
           throw;
        }
        finally
        {
           Console.WriteLine("After target call");
        }
    }
}
```

希望到这里你对动态代理是什么，它是如何工作的，有什么好处，有一个明确的概念。下一章我们深入到高级功能，plugging into，并影响生成代理类的过程。

## 继续阅读

[代理对象的类型](dynamicproxy-kinds-of-proxy-objects.md)
