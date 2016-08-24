---
layout: post
title: Default initialisation of TNT arrays and  STL vectors
tags: arrays TNT std::vector
---

_A reminder for DAV_

I had a weird bug the other day that was caused by mixing up the behaviour of **std::vector** and the **TNT::Array** types. The C++ vector from the Standard Template Library has default initialisation values for certain built in types like int, float, double etc. So if you create a **std::vector** like this:

{% highlight cpp %}
std::vector<double> MyVec(10);
{% endhighlight %}

It will have ten elements, all of type double, and they will be default initiaised to 0. Same is true for ints and floats. Strings receive an empty string: `""`. You can also specify another initialisation value if you don't want the deafult.

{% highlight cpp %}
std::vector<int> MyVecInts(10, 42);   // 10 ints all of value 42
{% endhighlight %}

But the TNT::Array type used in LSDTopoTools doesn't do this for you. If you don't initialise the array, it will contain garbage values from whatever happens to be in memory:

{% highlight cpp %}
TNT::Array2D<double> MyArray(100, 100);     // 100 x 100 array, no initialisation.
{% endhighlight %}

Instead I should have specified the initialisation value, like:

{% highlight cpp %}
TNT::Array2D<double> MyArray(100, 100, 0.0)     // 100 x 100 array, all zeros
{% endhighlight %}

Or initialised it with a for loop, or some other method.
