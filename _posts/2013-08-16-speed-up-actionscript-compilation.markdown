---
layout: post
title: "Speed up ActionScript3 compilation"
date: 2013-08-16
categories: linux actionscript
---

Each use of mxmlc is very slow because the whole code is recompiled.
The more code the more time it will take.
It would be perfect if the compilator took into process only changes.
It is possible and special tool comes with rescue.
In bin directory of Flex SDK you can find fcsh – it is shell which you can use to incremental compilation of your ActionScript code.

First you have to configure your system for using mxlmc and fcsh. Here you can find how to do this. Using of fcsh is very simple. Just open the terminal then go to project directory and run fcsh command. You will see something like this

{% highlight bash %}
Adobe Flex Compiler SHell (fcsh)
Version 4.6.0 build 23201
Copyright (c) 2004-2011 Adobe Systems, Inc. All rights reserved.

(fcsh)
{% endhighlight %}

Compile your code Main.as (or whatever you have) in this shell

{% highlight bash %}
mxmlc Main.as
{% endhighlight %}

and you will get output of compilation process and information about created target

{% highlight bash %}
fcsh: Assigned 1 as the compile target id
...
{% endhighlight %}

From here type compile and target id to start incremental compilation

{% highlight bash %}
compile 1
{% endhighlight %}

Unfortunately it is impossible to use arrow up to repeat command and tab to autocompletion compile command in this shell.
However there is a method to run it in background and use simple script to control compilation by sending messages through FIFO files.
There are several of these scripts on the Internet that make this, just search for fcsh wrapper.
I decided to write my own simple wrapper for educational and practical reasons.
You can find it [here][fcshc]
The best way to use it is place this in a makefile and type only _make_ when you need to recompile code.
The very simple content of the makefile should be something like this

{% highlight makefile %}
MXMLC=fcshc mxmlc
OPTIONS=-static-link-runtime-shared-libraries=true
MAIN=Main.as
SWF=output.swf

all:
	$(MXMLC) -sp $(OPTIONS) $(MAIN) -o $(SWF)

run:
	flashplayerdebugger $(SWF)
{% endhighlight %}

That’s all.

[fcshc]: http://github.com/gregoryprogrammer/fcshc

