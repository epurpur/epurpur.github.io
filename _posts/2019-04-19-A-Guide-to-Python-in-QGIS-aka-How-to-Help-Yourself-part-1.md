
This post is something I've been thinking about writing for a while and will be several parts. I was inspired to write it
by my own trials and tribulations, which are still ongoing, while working with the QGIS API, trying to programmatically do
stuff in QGIS instead of relying on available widgets and plugins. After spending many years working with ESRI/ArcGIS I have
made the transition to QGIS almost completely and have become a big fan. But this does not mean the transition has been
seemless. I have spent, and will probably continue to spend, many hours scouring the internet and especially Stack Overflow looking
for answers of how to use various classes, methods, attributes, etc. Especially in the beginning, but still quite often, I
feel like probably the dumbest person to have ever installed QGIS. I thought to myself many a time: "Am I an idiot or is this
not as easy as it appears?". After getting in contact with more QGIS users it appears that I am not alone. A great number of
people are struggling just as I am. One common thought I hear is: "I wish the documentation was a little more fleshed out,
provided more examples, was easier to follow...". This quelled my fears somewhat, it appeared I was not alone.

First, I would like to make it clear I am not taking shots at QGIS, the QGIS development team, or contributors. I am a big
QGIS fan and really appreciate all the hard work they've done to make QGIS what it is today. Before I dive into the ins and 
outs of how to help yourself I'd like to take a zen approach to the whole process and remind you, and myself, that QGIS is
a work in progress and is not flawless. In my experience, it appears that sometimes nobody has all the answers about why stuff
doesn't work, including said members of the development team. So take all this in stride as you press onward.

Honestly, I am a pretty novice python user and far from a QGIS API expert. Even still, I have figured out some tips and tricks 
that I wish I had known when I was learning the ropes and am attempting to synthesize this information, which I have
scraped from many sources, all in one place. I will also include quite a few examples to illustrate different points and show
how I attempt to attack the problem. Also, I'm going to break this into a series of blog posts to make them more digestible.

Lets begin from the top. What is the QGIS API? In the most basic terms, an API allows programs to communicate with one another.
In this case, QGIS and python are communicating with eachother. When you dive into the world of QGIS programming, you'll first
want to locate the documentation, which is available in several places. It took me a minute to figure out what is what. A 
really nice place to start is [The PyQGIS Developer's Cookbook](https://docs.qgis.org/testing/en/docs/pyqgis_developer_cookbook/).
This is both a tutorial and reference guide for the basics of PyQGIS programming. I reference it a lot.
Next, you need to know where to find QGIS API documentation, you can find this in two forms. In Python and C++. Use them,
bookmark them. Both versions of the documentation reference the same classes, but look slightly different. To be honest, I've
never written a single line of C++ code, but I find the most helpful part of the C++ documentation to be the inheritance 
diagrams. Here, you can visually see which classes are parents and children of others and I find this very helpful. Example:

![_config.yml]({{ site.baseurl }}/images/C++inheritance.png) . 

As seen above, there is a lot going on here. I chose QgsLayoutObject to show this class has several child classes. One of those
(QgsLayoutItem) has many children of its own. It also has parent classes (QObject is one of them). I'll touch on class
inheritance shortly, but any child class inherits methods from its parents.

For nearly everything else, I reference the Python Documentation. I find it more user friendly and easier to use. Let's take
a generic class for example and point out the features available. As an inexperienced user it took me some time to orient
myself to the documentation. I also highlighted where the parent class and base classes of the parent class are included.

![_config.yml]({{ site.baseurl }}/images/QgsLayoutObjectPython.png)

Next, for a given class you'll find everything else associated with that class. I'm going to specifically detail the class
methods and attributes. Class methods are basically just functions associated with a particular class. Attributes are simply
variables that belong to a class. QgsLayoutObject has many methods and attributes and you'll see them listed accordingly in
the documentation. The Python documentation is nice because everything is clickable and you can zoom around the page, clicking
on various buttons instead of scrolling. Let's take another class to show some more examples of how to interpret class methods
and attributes in the documentation.

Attributes are simpler. You can call them easily on an object to see value that object stores. In the next example, just look
at the last line as a demonstration of what an attribute looks like.

    layer = iface.activeLayer()
    provider = layer.dataProvider()
    extent = layer.extent()
    stats = provider.bandStatistics(1, QgsRasterBandStats.All)
    print(stats.minimumValue)                   #minimumValue is the attribute here
    
What I've done in this short script is taken the active layer in the project, which is a raster layer in this case, and called
some other functions to get the details about the pixel values in the raster layer. Finally I use the minimumValue attribute to
see the minimum pixel value in this raster layer. If you are coding in the QGIS Python Console, or its associated editor, you
can get some help as you go. Though the QGIS python console is not the best full-feature editor, I find it useful while coding
in QGIS. Let's illustrate the previous example a bit more. Long story short, the minimumValue attribute I used in the previous
example is an attribute of class QgsRasterBandStats. In the console I can quickly see which attributes are available to me like so: 

![_config.yml]({{ site.baseurl }}/images/QgsRasterBandStatsattributes.png)

Notice I left my line of code unfinished. The QGIS console auto-populated the possibilities for me. Attributes will always be
listed first, followed by methods. Other attributes available to me are max, min, mean, etc. 

Now lets talk about methods. Class methods are all the functions you can call from each class. In the symbology tab on any
raster layer, I can see a histogram of all the pixel values in that layer. I can also do this programmatically with the
QgsRasterInterface class. Here is a quick code example:

    layer = iface.activeLayer()
    provider = layer.dataProvider()
    provider.initHistogram(QgsRasterHistogram(),1,100)
    
I'll walk through each of the three methods I've used here. First, I created a layer object using the QgisInterface class and
the activeLayer() method. You might wonder why I used iface.activeLayer() instead of QgisInterface.activeLayer()? Honestly, I don't
know. But through examples and experience, iface is shorthand for QgisInterface. It is just one of the quirks of the
program. Anyway, in the QgisInterface documentation page you'll see the entry for the activeLayer() method:

![_config.yml]({{ site.baseurl }}/images/ifaceActiveLayer.png)

I've highlighted the arrow showing when you call this method, it returns a QgsMapLayer object. I can further demonstrate this:

![_config.yml]({{ site.baseurl }}/images/QgsMapLayerObject.png)

My active map layer is a raster layer, so I have a QgsRasterLayer object. The next attribute I called in the script is 
QgsMapLayer.dataProvider(). Using the same process I created an object called provider. Then called the .dataProvider() method
on my layer object which returns a QgsRasterDataProvider object. Remember, layer is really a QgsMapLayerObject. 
The third method is .initHistogram(). Here is where things get more interesting. Now that I have a QgsRasterDataProvider object
called provider. I find a method initHistogram(). Here is the entry for initHistogram() in the QgsRasterDataProvider docs:

![_config.yml]({{ site.baseurl }}/images/initHistogramblank.png)

Looks pretty empty to me. I chose this to demonstrate that QGIS documentation is an ongoing and imperfect process. Perhaps it
will be filled in by the time you read this. How am I supposed to know what this method does or how to use it? There are several
ways. I briefly touched on the concept of class inheritance. As I showed earlier, classes have parent and child classes. Child
classes lower on the food chain inherit methods from their parents. Luckily for us, QgsRasterDataProvider is a child of the 
QgsRasterInterface class, which also has an initHistogram() method with more descriptive information. Here is a screenshot from
QgsRasterInterface:

![_config.yml]({{ site.baseurl }}/images/QgsRasterInterfaceinitHistogram.png)

There is a lot more going on here. As you can see, initHistogram() takes many more arguments. Some are required, some
are not. The first argument of most (maybe all?) QGIS class methods will be "self". Because most objects are single instances of
a class, "self" will be the first argument. If you are unclear about this, I highly recommend [Corey Schafer's python
Object-Oriented programming videos](https://www.youtube.com/watch?v=ZDa-Z5JzLYM&vl=en). The 2nd argument is a histogram, followed
by what QGIS expects will be the object type you input for this argument, which is a QgsRasterHistogram. Meaning, first you 
must create a QGIS raster histogram. I give an example in the code below. Lastly, you see several of these arguments have 
default values so if you don't specify an input for this argument, the default value will be used. Here is an example of how 
to use the initHistogram() method:

    layer = iface.activeLayer()
    provider = layer.dataProvider()
    histogram = QgsRasterHistogram()                     #create an empty QgsRasterHistogram object
    provider.initHistogram(histogram,1,100)              #use the initHistogram() method on the QgsRasterDataProvider object
    
 Specifically looking at the .initHistogram() line, you can see I included 3 arguments. Really, 4 arguments, "self" is 
 implicitly the first argument. Walking through the documentation for the initHistogram() arguments, the first I included is
 my histogram, which is a QgsRasterHistogram object. Next is bandNo. This is the raster band number I'm working with. My raster
 layer only has one band. But your's might have multiple bands. In the docs, see that bandNo expects an integer as the input.
 Then I have 100 as the value for my binCount argument. The rest of the arguments I left blank and accepted the default value.
 A nice feature of the QGIS console/editor is that it auto-populates the arguments of various classes and methods as you are typing.
 This saves me quite a bit of time and explicitly states what you should be entering, instead of flipping back and forth between
 the documentation and your code. Here is what it looks like in action:
 
 ![_config.yml]({{ site.baseurl }}/images/initHistogramConsole.png) 
 
Let's step back once more. If you are like me, you probably frequently get lost while coding. In the short code example above, you
 might think: "Wait, what kind of object is provider, what am I working with here?". Fortunately, you can always ask for
 help. You can ask for help docs explicitly for a class, you also ask for help on your variables and other objects. If you are
 trying to remember what layer.dataProvider() actually is, just do this:
 
     help(layer.dataProvider())
 
 or call the variable itself.
 
     help(provider)

![_config.yml]({{ site.baseurl }}/images/helpprovider.png)

The help docs are long so I only included the first line or two. Now, I am nicely reminded that I am working with a 
QgsRasterDataProvider object. I can also quickly see the parent classes. I find the ability to call help docs for my 
variables enormously helpful.

I will end this post here. These are the basics of where to find help and how to interpret the documentation. In
the following posts in this series, I will dive further into the workflow of how to do various tasks in QGIS and how to 
orient yourself while coding. Stay tuned for more!


