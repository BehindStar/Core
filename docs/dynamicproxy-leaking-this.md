# 泄露（Leaking） `this`

## Problem

考虑这个示例接口/类对：

```csharp
public interface IFoo
{
    IFoo Bar();
}

public class Foo : IFoo
{
    public IFoo Bar()
    {
        return this;
    }
}
```

现在，使用目标为 `IFoo` 创建代理，并像这样使用：

```csharp
var foo = GetFoo(); // returns proxy
var bar = foo.Bar();
bar.Bar();
```

你看见这里的bug了吗？第二个调用没有在代理上执行，而是在目标对象自己上！代理正在泄露它的目标。

这个问题显然不会影响类代理（因为在这种情况下，代理和目标是相同的对象）。为什么动态代理不自己处理这种情况呢？因为没有一个简便的方式来处理。展示的例子是最不重要的，但是被代理对象可能以无数不同的方式泄露。可以作为返回对象的属性，可以作为事件的 sender 参数，可以将其赋给某些全局变量，可以将自己传递到它拥有的一个参数等等。动态代理不能预测任何一个，也不应该。

## 解决方案

某些情况下，经常没有太多的办法。最好是知道这样的问题存在，并了解其后果。在其他情况下，虽然，修复这个问题很简单。

```csharp
public class LeakingThisInterceptor:IInterceptor
{
    public void Intercept(IInvocation invocation)
    {
        invocation.Proceed();
        if (invocation.ReturnValue == invocation.InvocationTarget)
        {
            invocation.ReturnValue = invocation.Proxy;
        }
    }
}
```

增加一个拦截器（将其方法拦截器管道的最后），以将泄露的目标对象换回代理对象。它是那样简单。注意这个拦截器只针对上面例子的场景（目标通过返回值泄露）。对每种情况，你都需要一个专门的拦截器。
