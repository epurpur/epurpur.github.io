This is the 2nd part of a multi-part blog post that I'll continue to write about: How to help yourself using Python for QGIS 3
(PyQGIS 3). [In the first part](https://epurpur.github.io/A-Guide-to-Python-in-QGIS-aka-How-to-Help-Yourself-part-1/) I 
focused on basics and background information such as:
  -Where to find documentation
  -How to interpret the documentation
  -tips and tricks of getting more help
  -using the docs vs help in the QGIS python console
  
I will be applying these tactics in this post and following posts, showing examples of real world QGIS tools and workflows. I
was inspired to write these posts through my own frustrations in orienting myself to using Python with QGIS. While I think 
the QGIS development team has done a great job in providing an open source alternative to ESRI/ArcGIS, naturally there will be
gaps in documentation and so on as QGIS is a community effort, primarily relying on volunteers. I'm writing these posts for 
several reasons. First, I write them for myself as a way to attempt to better understand the solutions I figure out and
capture my frame of mind while I'm still current in the project. I assume I'll have similar issues in the future so hopefully
I can do my future self a favor and document the process as I go. Second, I think that if I am having these problems, other
QGIS users are too. Maybe I can save someone else some effort? Initially I thought no one would ever read my posts except me
but I have actually received so feedback from the community. So I know I am not alone.

In this post I am going to build on little bits of code I used in my previous blog post. This subject of this post will be:
How to make a plan of attack or How to address a PyQGIS issue. My workflow usually goes something like this:
  -Identify problem
  -Google it (ie: "Raster Calculator PyQGIS")
    -This usually leads me to a StackOverflow thread or QGIS documentation
  -Use someone else's code example, or as I build in experience, maybe adapt code from [my own knowledge repository](https://github.com/epurpur/PyQGIS-Scripts)
  -Look at PyQGIS classes documentation usually in [Python](https://qgis.org/pyqgis/master/) and maybe also in [C++](https://qgis.org/api/modules.html)
  -The usual coding process of trying, failing, re-writing, trying again
  -If I am really stumped, ask a question on StackOverflow
  
Now lets take an example. A while back I asked a question on StackOverflow. [I was working with a Raster layer and I wanted to
access the pixel values in that layer].(https://gis.stackexchange.com/questions/311047/pyqgis-raster-band-stats-access-pixel-values-of-raster-layer)
Here is the code I started from when I asked the question:
    layer = iface.activeLayer()
    print("Active Layer: ", layer.name())
    provider = layer.dataProvider()
    extent = layer.extent()
    stats = provider.bandStatistics(1, QgsRasterBandStats.All) 
    print("min value =", stats.minimumValue)
    print("max value =", stats.maximumValue)

I know how to do this in QGIS using various widgets. For a given raster layer, right click and go to "properties" > 
"symbology". Here, I can manually access the various pixel values in that raster layer. Here is a screenshot of what this
looks like via the "symbology" tab:

[insert symbology photo here]

Before I begin, I find it helpful to ask myself: "What am I working with?". This clues me in to what methods,
attributes, and as I get more experienced, what PyQGIS classes I might be working with. There are many types of things I
could be manipulating with python including layers, maps, databases, etc. In this case, the object I am working with is a
layer in my map. I like to get as much help as I can with my objects so I will run this help tool in the console:
    print(help(layer))                 #you can call help directly on variables you've created
    print(help(iface.activeLayer())    #you can call help this way too

In my project, I am working with a raster layer, which is an instance of the class [QgsRasterLayer](https://qgis.org/pyqgis/master/core/QgsRasterLayer.html).
Looking at the documentation I can now see all the methods, attributes, etc. available for that layer, including parent
classes and arguments needed if I use this class while coding. Next, I print the name of the layer (in the console), just
to make sure things are working:
    print("Active Layer: ", layer.name())

You can see I have a variable "layer" and called the method .name(). If I look through the QgsRasterLayer documentation, you
will not see the method .name() listed. So where did this come from? The answer is class inheritance. Looking at the class
inheritance diagram from the C++ documentation, I see that QgsVectorLayer inherits from a bunch of different classes. This
means, there are many methods and attributes a QgsVectorLayer object could use in addition to the ones listed in it's own
documentation:

[Insert QgsRasterLayer Inheritannce Diagram image]

QgsRasterLayer isn't a very complex inheritance diagram. Let's look at QgsVectorLayer for the sake of this example. Many attributes and methods are shared between QgsVectorLayer and QgsRasterLayer.

[Insert QgsVectorLayer Inheritance Diagram image]

These classes share many attributes and methods because they inherit from a common parent class, [QgsMapLayer](https://qgis.org/pyqgis/master/core/QgsMapLayer.html)
You can see, I used the name() method on my active layer, but where did this come from? Because class inheritance allows a
class to inherit from its parent classes, there are potentially many more methods and attributes available to each class than
are listed in the documentation. I found the name() method inside the parent class, [QgsMapLayer](https://qgis.org/pyqgis/master/core/QgsMapLayer.html#qgis.core.QgsMapLayer.name).
This step in the process feels a bit like a wild goose chase to me. QgsMapLayer only hase one parent class but In the case of 
QgsVectorLayer (and many other classes), it inherits from many classes which all contribute a lot of functionality. I really 
don't know an effective way to sift through all this except through experience. After a while, you just know what is 
available via various base classes. If you know a better way, let me know!

Anyway, the layer name is an aside. Back to the example. Here are the next two lines of code:
    provider = layer.dataProvider()
    extent = layer.extent()
QgsRasterLayer provides one of these methods, dataProvider(). But there is no information listed. How am I supposed to know
what this method does? Here you see a limitation of an open-source software project like QGIS. It is not someone's job to do 
make sure all the documentation is complete, like it is for someone at ESRI. Luckily, QgsMapLayer provides both this method
and extent() with more information. Lesson: it is worth taking a look at the base class(es) to see what information is
available.

[Insert extent() image]

[Insert [dataProvider() image]

The dataProvider method is particularly interesting because of what it returns, a QgsRasterDataProvider object, which I have
highlighted in the image above.

