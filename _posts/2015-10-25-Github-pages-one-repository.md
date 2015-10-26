---
layout: post
title: Managing repository websites win the same folder (by SMM)
excerpt: "How you manage a project website in GitHub--single folder."
categories: articles
tags: git, website
---

* Table of Contents
{:toc}

## Managing repository pages in Github (alternative approach)

In the previous post, I went through a method that involves keeping different directories for the two branches of your code. 
There is another method for keeping a single directory, which is detailed below. 

### Setting up a website branch

In guthub, the repository website is always read from a branch called *gh-pages*. 
The first thing you need to do to set up a repository website is to create this branch. 

You can go here: https://pages.github.com/ to see the instructions. 

In this example, we are going to make a project site, and start from scratch. 

Making a new branch in the repository is the easy bit!

### Setting up your repositories

We are going to follow this approach: http://lea.verou.me/2011/10/easily-keep-gh-pages-in-sync-with-master/

1. Make changes to the master and push them
2. First, check out the gh-pages branch
{% highlight console %}
$ home/Git_projects/LSDTT_book/
$ git git checkout origin/gh-pages -b gh-pages
{% endhighlight %}
3. Use the rebase function
{% highlight console %}
$ git branch -d master
{% endhighlight %}
8. Now you can push changes to your gh-pages branch from this repo without having to check it out each time
{% highlight console %}
$ git push -u origin gh-pages. 
{% endhighlight %}
