---
layout: post
title: "ActionScript3 code compilation in Linux"
date: 2013-07-02
categories: linux actionscript
---

Despite appearances it’s very easy.
All you need is Flex SDK which is available for download on Adobe site.
There are sufficient tools in this SDK to compile ActionScript 3 code into single SWF file.
Furthermore there is no problem to embed resource files (MP3, PNG) with it.
I assume that you have Linux operating system and you are familiar with using the terminal.
Obviously you have to know ActionScript 3 basics at least.


### Workspace
Create directories for sdk, projects and scripts

{% highlight bash %}
mkdir -p /home/$USER/AS3/sdk
mkdir /home/$USER/AS3/projects
mkdir /home/$USER/AS3/scripts
{% endhighlight %}

Download Flex SDK (latest version is 4.6).
You can find it [here][flex-sdk-download]
Unzip it into sdk directory

{% highlight bash %}
unzip flex_sdk_4.6.zip -d /home/$USER/AS3/sdk
{% endhighlight %}

For convinient work add some paths to shell rc script. Example for bash

{% highlight bash %}
echo "PATH=\$PATH:/home/$USER/AS3/sdk/bin" >> ~/.bashrc
echo "PATH=\$PATH:/home/$USER/AS3/sdk/runtimes/player/11.1/lnx" >> ~/.bashrc
{% endhighlight %}

Untar Flash Player

{% highlight bash %}
cd /home/$USER/AS3/sdk/runtimes/player/11.1/lnx
tar zxf flashplayerdebugger.tar.gz
{% endhighlight %}

Reload shell rc script

{% highlight bash %}
source ~/.bashrc
{% endhighlight %}


### Code
Create a directory for test code and file named Main.as in there. Fill it with sample code I provide below.
For now you can just use your favorite text editor as EDITOR but in the future I will
show you how to fit vim for comfortable work with ActionScript.

{% highlight bash %}
mkdir /home/$USER/AS3/projects/Test 
EDITOR /home/$USER/AS3/projects/Test/Main.as
{% endhighlight %}

Sample code

{% highlight as3 %}
package {
	import flash.display.Sprite;

	public class Main extends Sprite
	{
		public function Main():void
		{
			with(graphics) {
				lineStyle(1, 0xff7979ff);
				moveTo(10, 10);
				lineTo(100, 10);
				lineTo(100, 100);
				lineTo(10, 100);
				lineTo(10, 10);
			}
		}
	}

}
{% endhighlight %}


### Compilation
For now we are ready to compile test code

{% highlight bash %}
cd /home/$USER/AS3/projects/Test/
mxmlc Main.as
{% endhighlight %}

During compilation you will get warning

{% highlight text %}
This compilation unit did not have a factoryClass specified
in Frame metadata to load the configured runtime shared libraries.
To compile without runtime shared libraries either set 
the -static-link-runtime-shared-libraries option to true 
or remove the -runtime-shared-libraries option.
{% endhighlight %}

If you don’t want to see this warning just add option to compilation

{% highlight bash %}
mxmlc -static-link-runtime-shared-libraries=true Main.as
{% endhighlight %}

Anyway if there weren’t errors, file named Main.swf should appear in the Test folder. You can run it by command

{% highlight bash %}
flashplayerdebugger Main.swf
{% endhighlight %}

The result should be Adobe Flash Player window running Main.swf file and there will be blue rectangle on white background.
As you can see the compilation isn’t very fast and each recompilation of the same code (or with small changes) takes the same time.
In the next post I will show you how to speed up this process by using fcsh shell with fcshc script.


[flex-sdk-download]: http://www.adobe.com/devnet/flex/flex-sdk-download.html
