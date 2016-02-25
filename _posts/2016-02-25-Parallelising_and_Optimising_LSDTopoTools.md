---
layout: post
title: Parallelising and Optimising LSDTopoTools
categories: articles
tags: openmp, parallelisation, hpc
---

Some notes and experiences from scaling LSDTopoTools code to parallel computing facilities (HPC). I'm using the OpenMP standard, which is widely used and is applicable to all kinds of computing architectures. It will also work on your desktop pc/laptop if you have a multicore CPU.

### Identifying hotspots and 'parallelisable' functions

First, profile your code to find where the bottlenecks are. You could use gprof for this. Before you compile the code however, remember to enable the profiling flags `-pg` in the makefile. When you run the compiled code again, you will get the gmon.out file. This contains all the profiling data. To generate some useable output from this, run:

{% highlight console %}
gprof MyLSDExecutable.out gmon.out > analysis.txt
{% endhighlight %}

The anlysis text file will give you some output like so:

{% highlight console %}
Flat profile:

Each sample counts as 0.01 seconds.
  %   cumulative   self              self     total           
 time   seconds   seconds    calls  ms/call  ms/call  name    
 77.71   5993.06  5993.06   234993    25.50    25.50  LSDCatchmentModel::qroute()
 17.63   7352.97  1359.91   234993     5.79     5.79  LSDCatchmentModel::depth_update()
  3.58   7629.10   276.13    46998     5.88     5.88  LSDCatchmentModel::scan_area()
  1.02   7707.40    78.29   234993     0.33     0.34  LSDCatchmentModel::catchment_water_input_and_hydrology(double)
  0.03   7709.43     2.03   234993     0.01     0.01  LSDCatchmentModel::water_flux_out(double)
  0.01   7710.29     0.86        7   122.86   191.43  LSDCatchmentModel::get_area4()
  ...
{% endhighlight %}

The analysis tells us that there is one function in this code taking up the bulk of the compute time: `qroute()`. (In this case it's a water-routing algorithm. Followed by a water depth update function and an area scanning function. This is quite a good scenario (we don't have to parallelise dozens of function to get a good return).

### Parallelisation

I used the OpenMP library to parallelise the code. OpenMP is a magical set of preprocessor directives that you add to sections of your code to run them in parallel. It does some clever stuff to your code when it compiles. For some light bedtime reading, you could consult the 568-page specification document: http://www.openmp.org/mp-documents/openmp-4.5.pdf.

Firstly, you need to include the `<omp.h>` header file in one of your source files. 

Secondly, you need to identify the part of the function that can be parallelised. I use the `down_scan()` function as a simple example. (qroute() is not so straightforward). This function has a for loop that iterates over every element in a 2D raster matrix. It then checks to see if the water depth  is greater than zero, and sets a value in another array based on this test. This loop is easily parallelisable because the iterations do not depend on each other, and could be calculated in any order. 

{% highlight cpp %}
  #pragma omp parallel for
  for (int j=1; j <= jmax; j++)
  {
    int inc = 1;
    for (int i=1; i <= imax; i++)
    {
      // zero scan bit..
      down_scan[j][i] = 0;
      // and work out scanned area. // TO DO (DAV) there is some out-of-bounds indexing going on here, check carefully!
      if (water_depth[i][j] > 0
          || water_depth[i][j - 1] > 0
          || water_depth[i][j + 1] > 0
          || water_depth[i - 1][j] > 0
          || water_depth[i - 1][j - 1] > 0
          || water_depth[i - 1][j + 1] > 0
          || water_depth[i + 1][j - 1] > 0
          || water_depth[i + 1][j + 1] > 0
          || water_depth[i + 1][j] > 0
          )
      {
        down_scan[j][inc] = i;
        inc++;
      }
    }
  }

{% endhighlight %}

When the code runs with OpenMP enabled, the iterations in this loop will be distributed to different threads and cpus. (More on this later). I put in similar `#pragma omp parallel for` lines in the two other expensive functions found by the profiler. You will need to check which loops are suitable for parallelisation (This is the tricky bit - not loop iterations are independent of each other and parallelise easily. It is best to consult a brief tutorial on OpenMP for how to do this, and look at other functioning examples. So I won't go into this in any more details. Here are some useful tutorials:

http://www.drdobbs.com/getting-started-with-openmp/212501973
https://computing.llnl.gov/tutorials/openMP/

ARCHER (the UK national supercomputing centre) also list details of upcoming training courses, and you can look through their training slides and exercises here:

http://www.archer.ac.uk/training/course-material/2015/12/ShMem_OpenMP_York/index.php

### Compiling and running

You need to add just one extra flag to your makefile: `-fopenmp` (for gcc users). That's it. You probably want to turn on an optimisation flag as well (e.g. -O2), unless you are debugging. 

Once compiled, you can run the executable on your computer/cluster. All you need to do beforehand is set the number of threads that you want the code to use. (It can't always detect this from your environment, so best to set it explicitly). In the linux terminal this is done like so (I have no idea how you do it in Windows).

{% highlight console %}
export OMP_NUM_THREADS=8
{% endhighlight %}

Where 8 should usually be the same as the number of available cores on your machine. Usually this means 1 physical processor, which has several physical cores on the chip. If you are running this in some sort of cluster-environment, OpenMP is limited to accessing a single compute node and the memory associated with that node. You may have more than one 'compute node' available, but an OpenMP application cannot distribute a program across multiple separate nodes. (Read the Dr Dobbs link above if this is unclear.) 

Then just run the executable as normal. You can check the usage of your cpus using the `top` command in linux. Then press 1 to get a breakdown of usage by CPU. You should hopefully see that all of your cpus (cores) are now in use, compared to just one that would have been in use when running the code serially.

(More details to come.)







