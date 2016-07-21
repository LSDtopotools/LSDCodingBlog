---
layout: post
title: Can you overload Python functions?
tags: function-overloading python LSDPlottingTools stop-thinking-like-a-c++-programmer
---

No. You cannot strictly speaking, overload Python functions. One reason being is that types are not specified in function definitions (the language is [Dynamically Typed](http://python-history.blogspot.com/2009/02/pythons-use-of-dynamic-typing.html) anyway, and accepts optional function arguments.)

If you want different function behaviour based on different types, you have to introduce some conditional logic. I wanted to 'overload' this function so it was either supplied a filepath to a raster, which it then converted to an array, or would be supplied a NumPy array directly:

{% highlight python %}
def DrapedOverHillshade(FileName, DrapeName, thiscmap='gray',drape_cmap='gray',
                            colorbarlabel='Elevation in meters',clim_val = (0,0),
                            drape_alpha = 0.6, ShowColorbar = False):
                            
{% endhighlight %}

So `DrapeName` could either be type `numpy.ndarray` or `str`. At the appropriate point in the function, I added a check to find what type had actually been passed, then called a relevant function if a file -> array conversion was needed.

{% highlight python %}
if isinstance(DrapeName, str):
  raster_drape = LSDMap_IO.ReadRasterArrayBlocks(DrapeName)
elif isinstance(DrapeName, np.ndarray):
  raster_drape = DrapeName
else:
  print "DrapeName supplied is of type: ", type(DrapeName)
  raise ValueError('DrapeName must either be a string to a filename, \
  or a numpy ndarray type. Please try again.')
{% endhighlight %}

Not this uses `isinstance(object, type)` rather than something which might be more intuitive like `if type(DrapeName) is type` for example. This is because *type()* only checks for the *exact* type specified - and would ignore any subtype or subclass of the object specified. 

It would also have been possible to not check the type at all, and just use a `try, catch` statement, first treating *DrapeArray* as a string, then as an array. ([And actually, some people might consider this to be the more pythonic way of doing it...](http://stackoverflow.com/questions/1549801/differences-between-isinstance-and-type-in-python))
