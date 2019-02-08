I've been struggling off and on for literally months trying to create and export a print layout using Python for QGIS 3. Or
PyQGIS 3 for short. I have finally figured out may of the ins and outs of the process and hopefully this will serve
as a guide to save someone else a lot of head scratching and save a lot of time. 

I greatly underestimated this process so don't take it lightly! Due to the near unlimited customization one print layout can
have, there are a lot of small details I'll go into and show examples for. You can mix and match for your needs. I'm also
going to dive into the documentation to show you how I referenced various class methods, attributes, etc.

    project = QgsProject.instance()         
    manager = project.layoutManager()       
    layout = QgsPrintLayout(project)        
    layoutName = "PrintLayout"
    layout.initializeDefaults()             #initializes default settings for blank print layout canvas
    layout.setname(layoutName)
    manager.addLayout(layout)

Let's discuss what I just did. I created a project, which is a reference to the current Qgs project we are working in.
The manager object holds the layout manager (layoutManager) for the current project. I create a layout and initalize the
default settings (size, layout, etc) for the print layout. 

If you run the above code, look under "Project" > "Print Layouts" and you'll see a new print layout, which is currently empty.
Now the fun begins. We need to add stuff to the print layout. Common things people add to the print layout include a map,
legend, labels, scale bar, north arrow, etc. I am just going to address the map, legend, and labels in this example. Here is
a reference to the documentation for the [QgsLayoutItem class](https://qgis.org/api/classQgsLayoutItem.html). You'll see this
class is a constructor for all the QgsLayoutItems of which there are many. Here is the inheritance diagram:

[QgsLayoutItems inheritance diagram]

Each item has its own attributes, methods, etc. that were difficult to navigate (for me at least). Let's start by adding a
map object to the print layout. See the following code:

    map = QgsLayoutItemMap(layout)
    map.setRect(20, 20, 20, 20)                                     #I have no idea what this does, but it is necessary
    #Set Map Extent
    rectangle = QgsRectangle(1355502, -46398, 1734534, 137094)      #defines map extent using map coordinates
    map.setExtent(rectangle)
    layout.addLayoutItem(map)
    #Move & Resize map on print layout canvas
    map.attemptMove(QgsLayoutPoint(5, 27, QgsUnitTypes.LayoutMillimeters))
    map.attemptResize(QgsLayoutSize(239, 178, QgsUnitTypes.LayoutMillimeters))
    
So in the previous code I created a map, which is a QgsLayoutItemMap object. My apologies but the map.setRect() method 
confuses me and to be honest, I can't explain what it does or why its there. But it is necessary for the map to exist. 
The arguments (20, 20, 20, 20) seem to have no effect on the map but without calling this method. You'll get an error.
Next, I set the extent of the map. I have created a QgsRectangle object and manually defined the extent using map coordinates
from my project. You can alternately define the extent using the following, which sets the extent to that of the interface's
map canvas, allowing you to change it dyamically.

    canvas = iface.mapCanvas()
    map.setExtent(canvas.extent())
    layout.addLayoutItem(map)
    
Lastly, I move and resize the map object. Both methods' arguments are pretty self explanatory. I move and resize the map
object in millimeters. You can choose to work in inches or other units if you desire doing something like:

    map.attemptMove(QgsLayoutPoints(1, 2, QgsUnitTypes.LayoutInches))
    

