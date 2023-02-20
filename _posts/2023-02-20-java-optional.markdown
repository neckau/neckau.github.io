---
layout: post
title:  "Java中何时使用Optional为益"
date:   2023-02-20 8:00:00 +0800
categories: jekyll update
---

在Java中Optional主要可用在三处：

1. 类属性
2. 方法参数
3. 方法返回值

首先，Optional的作用是明示代码使用者主动考虑空指针的情况，它并不是null的替代品，试图使用Optional完全替换程序中的null是无意义的。所得不过是给null换了个别名。该判断null的地方也就是换了个判断逻辑，不仅一句话没省，还白白多了对象封装。想消除null最好的做法是根本不要产生null，用类似Empty或者Default对象替代它。

基于这一点，Optional最佳的使用位置是方法的返回值处。返回Optional提示别的程序员，使用我的方法需要考虑结果为null的情况。

那么在方法参数中用Optional呢？其意为告知对方，我的方法可以接受你传null？不合时宜。第一，不接受null才需要特殊说明。第二，此时不如用@Nullable。第三，别人可能会传一个为null的Optional对象，于是需要判断的地方比之前还更多了。

最后，类属性使用Optional更加无意义。类属性并不对外使用，本身应该封装在方法里的东西，如果不希望它为空，就在自己封装的代码里处理好。

另外，在Intellij中我们可以看到，如果使用Optional包装类的属性或方法参数，会提示警告“Optional used as a field or parameter”。可见JetBrain也是同意这个观点的。不过万事不绝对，就好像这句提示之所以是一个warning而不是error，因为确实会有特殊情况。

关于Optional可以用在哪里有很多讨论，方法返回值是最佳使用位置，以及类属性不应该用Optional这是无可争议的。讨论的焦点主要在方法参数是否可以使用Optional，类似的讨论可以参考这里：
https://stackoverflow.com/questions/31922866/why-should-java-8s-optional-not-be-used-in-arguments
