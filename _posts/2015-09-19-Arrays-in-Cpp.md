---
Title: Arrays in C++ with TNT
Layout: default
---

## Multidimensional Arrays in C++
C++ does not have a native way of doing multidimensional arrays like some other languages do. There are several ways to workaround this though. Here are some ideas:

### Nested Vector method
Using the `std::vector` object, nest one vector within another like so:
{% highlight cpp %}

#include <vector>

std::vector< std::vector<int> > MyArray;
{% endhighlight %}

Note: the above array has not been initialised with anything, nor does it have a predefined size at compile time as vectors can be allocated dynamically, therefore so can our makeshift 2D Array. It's effectively a vector of vectors with type int.

### Nested array (`C++11`) method
C++11 has a new feature in the standard library: `std::array`. Unfortunately it also does not support multidimenional arrays, but you can nest one within another.
{% highlight cpp %}
#include <array>

std::array< std::array<float, 3>, 3 > MyArray  = { { {5, 8, 2}, {8, 3, 1}, {5, 3, 9} } };
{% endhighlight %}

This is a 3x3 array as we've specified the dimensions in the declaration. We've also intialised the values in the array. Only available with C++11.


## Using the Template Numerical Toolkit (TNT)
Eventually, the workarounds above can get a bit tiresome, or difficult to read using higher dimensional arrays. In LSDTopoTools, we use a library called the Template Numerical Toolkit, which adds support for multidimensional arrays, without the awkward looking syntax used above. 

## Other Methods
You could define your own template (good luck!) or use C-style arrays (ugh...).
