I was recently working on a project in QGIS 3 with a member of UVA Health's Oncology
department. This person wanted to take a set of patient data (after identifying info had
been removed) and after doing some other stuff, apply a graduated color scheme to the
results, shading them from light to dark based on intensity.

You can find a sample dataset for this project here:
https://github.com/epurpur/PyQGIS-Scripts/blob/master/TestZipCodes.zip

This is a shapefile with a handful of zip codes in Virginia.

After looking around online I found some documentation and examples, but some were for
older versions of the software. But I did find a nice current example in the PyQGIS 
Developer's Cookbook:
https://docs.qgis.org/testing/en/docs/pyqgis_developer_cookbook/vector.html

My code an example is basically a simplified version of the above link with some more 
description of particular features. Anyway, lets start from the top. If you've downloaded
the sample layer, you have a shapefile with zip codes and in that layer's attribute table
is a column called 'Patient_Da'. This is short for "Patient Data" and shows a count
of number of patients by zip code. Here is the full code:


uri = '/Users/ep9k/Desktop/TestZipCodes/TestZipCodes.shp'
join_layer = iface.addVectorLayer(uri, 'Patients by Zip Code', 'ogr')
target_field = 'Patient_Da'

﻿def apply_graduated_symbology():
    """Creates Symbology for each value in range of values. 
        Values are # of patients per zip code.
        Hard codes min value, max value, symbol (color), and label for each range of 
        values. Then QgsSymbolRenderer takes field from attribute table and item from 
        myRangeList and applies them to join_layer. Color values are hex codes, 
        in a graduated fashion from light pink to black depending on intensity"""
        
    myRangeList = []

    symbol = QgsSymbol.defaultSymbol(join_layer.geometryType())     
    symbol.setColor(QColor("#f5c9c9"))                              
    myRange = QgsRendererRange(0, 3, symbol, 'Group 1')                   
    myRangeList.append(myRange)                                     

    symbol = QgsSymbol.defaultSymbol(join_layer.geometryType())
    symbol.setColor(QColor("#000000"))
    myRange = QgsRendererRange(3.1, 6, symbol, 'Group 2')
    myRangeList.append(myRange)

    myRenderer = QgsGraduatedSymbolRenderer(target_field, myRangeList)  
    myRenderer.setMode(QgsGraduatedSymbolRenderer.Custom)               

    join_layer.setRenderer(myRenderer)                                  
    
    print(f"Graduated color scheme applied")

apply_graduated_symbology()

First, I want to show you what this would look like using the "symbology" tab of the GUI
in QGIS. This is simple enough to do.



![_config.yml]({{ site.baseurl }}/images/GraduatedSymbology.png)

In the first three lines I created a uri which is the path to where my file is stored.
You should modify your pathname accordingly. Then, I create the join_layer object and 
add the layer to my map. Finally, the target_field object is the 'Patient_Da' column in
the attribute table of my join_layer, which is the sample data set:


uri = '/Users/ep9k/Desktop/TestZipCodes/TestZipCodes.shp'
join_layer = iface.addVectorLayer(uri, 'Patients by Zip Code', 'ogr')
target_field = 'Patient_Da'


Now, I created a function called apply_graduated_symbology() with no arguments. You can 
see from my docstring that this creates symbology for each value in a range of values. I
specify this range of values and create a custom symbol for each. 
In the next few lines, I create a list object called myRangeList and set it to an empty
list. This will hold the symbol for each range of values. 

Next, I create a variable called symbol. symbol first stores the geometry type of the
layer, which in this case is a polygon. Yours might be a line or point (marker). Insert
a print statement here if you want to determine what yours is. Then, I set the color
for this geometry type. I manually specified this color using a hex string ("f5c9c9"). 
This hex string is a pinkish color.

Then, create myRange. This creates a QgsRendererRange which takes the following arguments:
(min value, max value, color, label). You can see that I hard coded the min, max, and
label. The color I already defined and stored in symbol. You can do something more
advanced here to calculate the min and max if needed. Lastly, I append this to the
myRangeList

 
    symbol = QgsSymbol.defaultSymbol(join_layer.geometryType())     
    symbol.setColor(QColor("#f5c9c9"))                              
    myRange = QgsRendererRange(0, 3, symbol, 'Group 1')                   
    myRangeList.append(myRange)


Notice I did this sequence twice, for two ranges of values. The first is for 0-3 patients,
the second is for 4-6 patients. You can add as many breaks as you like here. I am only 
using two for this simple example. Anyway, myRangeList now has two objects in it. The last
few lines implement the code we've written. myRenderer creates an object which stores the 
QgsGraduatedSymbolRenderer object with the field name (target_field from above) and 
myRangeList (which stores the QgsRendererRange values for each break as arguments). Then
we define the mode as "Custom". You could also use other modes such as Natural Breaks or
Jenks.
Lastly, you must render your layer and apply the graduated symbology. 
join_layer.setRenderer(myRenderer) does this. Take a look at the last few lines of code
again:


    myRenderer = QgsGraduatedSymbolRenderer(target_field, myRangeList)  
    myRenderer.setMode(QgsGraduatedSymbolRenderer.Custom)               

    join_layer.setRenderer(myRenderer)                                  
    
    print("Graduated color scheme applied")
    
apply_graduated_symbology()
    
    
I finish with a print statement telling me this function ran and then execute the function
outside the function block.

So thats really all there is to it. I went through some head scratching moments trying to
follow documentation and various StackOverflow threads and eventually put the pieces
together. I wrote this because I really couldn't find a great example resource to use
so hopefully this serves as that for you.

