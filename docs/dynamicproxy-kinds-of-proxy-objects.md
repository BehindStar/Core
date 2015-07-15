# 代理对象的类型

Out of the box, 动态代理提供了几种代理对象以供使用。它们分为两大类：

## 基于继承的：

基于继承的代理通过继承一个代理类创建。 代理拦截对类的虚成员的调用，并转发它们到基实现。在这种情况下，代理和被代理的对象是一体的。这也意味着不能为已存在的对象创建基于继承的代理。在动态代理中有一个基于继承的代理类型。

* 类代理 - 为类创建基于继承的代理。只有类的虚成员可以被拦截。

## 基于组合的：

基于组合的代理是一个继承自被代理类/实现被代理接口并（可选）转发被拦截的调用到目标对象的新对象。动态代理公开了以下基于组合的代理类型：

* 有目标类代理 - 这种代理类型面向类。这不是一个完美的代理，如果类有不能被拦截的非虚拟方法或公共字段，giving users of the proxy inconsistent view of the state of the object. 因此，应该慎用。

* 无目标接口代理 - 这种代理类型面向接口。不需要提供目标对象。相反，拦截器需要为代理提供所有成员的实现。

* 有目标接口代理 - 顾名思义包装对象实现所选接口，转发这些接口的调用到目标对象。

* 有目标接口接口代理 - 这种代理是其他两种代理类型的混合体类型。它允许，但不需要提供目标对象。也允许在代理的生命期内交换目标。它不依赖于代理目标的一个类型，因此一个代理类型可以被不同的目标类型重用，只要目标类型实现了目标接口。

## 外部资源

* [Tutorial on DynamicProxy discussing with examples all kinds of proxies](http://kozmic.pl/dynamic-proxy-tutorial/)
* [Castle Dynamic Proxy not intercepting method calls when invoked from within the class](http://stackoverflow.com/questions/6633914/castle-dynamic-proxy-not-intercepting-method-calls-when-invoked-from-within-the/)
* [Why won't DynamicProxy's interceptor get called for *each* virtual method call?](http://stackoverflow.com/questions/2153019/why-wont-dynamicproxys-interceptor-get-called-for-each-virtual-method-call)
