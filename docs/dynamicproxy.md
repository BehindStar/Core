# Castle 动态代理（DynamicProxy）

动态代理为对象生成代理，这样就可以为它们透明的添加或修改行为，提供前/后处理和许多其它东西。Castle Windsor 使用代理来实现拦截能力和强类型工厂。[NHibernate](http://nhforge.org) 使用代理提供延迟加载能力。[Moq](http://moq.me) 和 [Rhino Mocks](http://www.ayende.com/projects/rhino-mocks.aspx) 使用代理提供模拟能力。这些仅是一些广为人知的框架和流行用法。

* 如果是第一次接触动态代理可以阅读 [简单介绍](dynamicproxy-introduction.md)。
* 浏览库中核心类型的描述。
* 深入高级内容，详细讨论：
  * [代理对象的类型](dynamicproxy-kinds-of-proxy-objects.md)
  * [Leaking this](dynamicproxy-leaking-this.md)
  * [Make proxy generation hooks purely functional](dynamicproxy-generation-hook-pure-function.md)
  * [Overriding Equals/GetHashCode on proxy generation hook](dynamicproxy-generation-hook-override-equals-gethashcode.md)
  * [Make your supporting classes serializable](dynamicproxy-serializable-types.md)
  * [Use proxy generation hooks and interceptor selectors for fine grained control](dynamicproxy-fine-grained-control.md)
  * [SRP applies to interceptors](dynamicproxy-srp-applies-to-interceptors.md)

:information_source: **Where is `Castle.DynamicProxy.dll`?:** DynamicProxy used to live in its own assembly. As part of changes in version 2.5 it was merged into `Castle.Core.dll` and that's where you'll find it.

:warning: **Use of a single ProxyGenerator's instance:** If you have a long running process (web site, windows service, etc.) and you have to create many dynamic proxies, you should make sure to reuse the same ProxyGenerator instance.  If not, be aware that you will then bypass the caching mechanism.  Side effects are high CPU usage and constant increase in memory consumption.

## See also

* [Castle DictionaryAdapter](dictionaryadapter.md)
