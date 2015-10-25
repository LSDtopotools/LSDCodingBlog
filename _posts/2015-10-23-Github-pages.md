---
layout: post
title: Managing repository websites
tags: git, website
---

## Managing repository pages in Github

Github enables you to have website for your repositories. 
Managing these websites takes a little bit of getting used to, because you will have to be familiar with banches in git. 
The post explains one way to manage a master branch for the actual software and a gh-pages branch for the website. 

### Setting up a website branch

In guthub, the repository website is always read from a branch called *gh-pages*. 
The first thing you need to do to set up a repository website is to create this branch. 

You can go here: https://pages.github.com/ to see the instructions. 

In this example, we are going to make a project site, and start from scratch. 

Making a new branch in the repository is the easy bit!

### Setting up your repositories

Once you have created a branch, you need to manage both the master and the gh-pages branches. 
You don't want them sitting in the same place!!

We are going to follow the instructions here: https://gist.github.com/chrisjacob/833223

. First, make sure your master branch is up to date on github and then delete it locally. You are going to clone it into a master subdirectory. 
I am going to do this with the LSDTT_book repository. 

. In the LSDTT_book repoository, make two directories: *master* and *gh-pages*.

. Now clone the main repositopry into the *master* repo from the LSDTT_book directory

{% highlight console %}
$ pwd
home/Git_projects/LSDTT_book/
$ git clone https://github.com/LSDtopotools/LSDTT_book.git master
{% endhighlight %}

. Now cline the repo *AGAIN*, but this time into the gh-pages directory

{% highlight console %}
$ pwd
home/Git_projects/LSDTT_book/
$ git clone https://github.com/LSDtopotools/LSDTT_book.git gh-pages
{% endhighlight %}
