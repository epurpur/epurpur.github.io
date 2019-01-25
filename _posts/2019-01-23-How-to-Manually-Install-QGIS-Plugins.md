
I recently took up a side project working with a colleague to add some functionality to an existing QGIS plugin. This led
me down a road of exploration, frustration, hand wringing, and ultimately learning. First, I'll quickly address what a plugin
is.  Basically a plugin is a bit of extra functionality, probably written by a third party, which you can download and use in
QGIS. [The official list of qgis plugins are availabe here](https://plugins.qgis.org/).

Part of the fun of an open source project like QGIS is that you can write your own plugins. If you create a plugin that serves
a purpose not covered by other QGIS tools (or other QGIS plugins), you can make it publicly available via the official
repository. I won't go into plugin creation much here, that is a potential topic for a future blog post. Today we are talking
about how to install a plugin manually on your local machine. Why would you do this? I can think of 2 good reasons but I am
sure there are more. First, if you have created a plugin that you want to use personally or within your organization but don't
want to share it publicly. Second, if you are developing a plugin or adding to an existing plugin, you can create your own
development version before rolling it out for everyone.

So let's get to it. How does one install a QGIS plugin locally and manually? I figured I'd need to learn about QGIS plugins in
general in order to do my project, so I started with my go-to resource, [The PyQGIS Programmer's Guide: Extending QGIS 3 with
PyQGIS](https://locatepress.com/ppg3) by Gary Sherman, the original founder of QGIS. The book has a full chapter dedicated to
writing plugins as well as parts of other chapters dedicated to various aspects of plugin architecture, etc. If you don't have
the book, Gary Johnson himself has created the "Plugin Builder", which is a plugin itself, to create plugins. You add some
metadata and the Plugin Builder and voila! You have a QGIS plugin. Most of the pieces are there but I encountered some issues
actually implementing this so I wanted to share my experience as well as some pitfalls you may want to look out for.

First, plugins are managed, installed, uninstalled, etc. via the Plugin Manager, which looks like this.
![_config.yml]({{ site.baseurl }}/images/PluginManagerImage.png)

When you go through the Plugin Builder steps, at the end you'll get a couple important pieces of information to help guide
you further.

![_config.yml]({{ site.baseurl }}/images/PluginBuilderPopup.png)
![_config.yml]({{ site.baseurl }}/images/pyrcc5warning.png)

As the warning message says, you must manually compile the resources.qrc file in your plugin directory before installing 
your plugin. Both the book and QGIS documentation ask you to open the terminal (Mac) or shell (Windows), navigate to where
your new plugin folder has been created by the Plugin Builder, and run the following command:

pyrcc5 -o resources_rc.py resources.qrc

I honestly don't know why, but I do know this DOES NOT WORK.  What you really should run is:

pyrcc5 -o resources.py resources.qrc

Obviously, QGIS reads the plugins folder and looks for resources.py to read from. Not resources_rc.py. You might also look
at this command and wonder, what is this pyrcc5 business? Here is the [pyrcc5 documentation](http://pyqt.sourceforge.net/Docs/PyQt5/resources.html), but in short, this reads resources.qrc and generates a python
module that only needs to be imported by the application. 

Write about edit metadata.txt next...



