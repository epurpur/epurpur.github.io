![_config.yml]({{ site.baseurl }}/images/MapsimiseImages/brelogo.png)

MAPSIMISE USER MANUAL


Navigation Menu:


Mapsimise Organization Homepage: [https://boonerealestate.useast.mapsimise.com](https://boonerealestate.useast.mapsimise.com/)

Username: epurpur@gmail.com
Password: Battlebot4$

The landing page looks like this:
![_config.yml]({{ site.baseurl }}/images/MapsimiseImages/mainscreen.png)

We only have one map with 1 view. The views are basically just different ways of looking at the same
data but for our sake I didn't feel like different views were necessary. 

Salesforce Connector
This is where we connect to Salesforce. **Interfering with this or deleting/reconnecting will 
remove all the data filters I have created. No touchy!**
![_config.yml]({{ site.baseurl }}/images/MapsimiseImages/salesforceconnector.png)

Billing
The billing page shows your subscription and payment information.
![_config.yml]({{ site.baseurl }}/images/MapsimiseImages/billingpage.png)




Filtering Data

There are several ways of filtering data.

Data Filters

Data filters are placed on the data via the attribute table of the dataset. For example: View all 
records which have the property type of "Home". You can and probably should double up filters. For example, you can use a data filter and a geographical filter on the data. 

To select a data filter click on the three horizontal bars button next to the 'master list' layer in the bottom of the main map screen. ![_config.yml]({{ site.baseurl }}/images/MapsimiseImages/toolbar.png) 

Click 'data filter' and you'll see a dropdown bar which allows you to select different options. **You 
should always view the data with a data filter**. Otherwise, it'll take a while for the program to load 
230,000 records. There are three custom data filters, which I created that you should not delete, along with a few others. Here are the data filter options:

<em> No Data Filter </em>: Just as it says, no filtering of the dataset. This views all records.

<em> New Data Filter </em>: Create a custon new view of the data. I imagine both you and I will use this 
because we are always tweaking the way we look at the data.

<em> Condo Filter </em>: Shows records with property type of "condo", "townhome", or "condo/townhome". 
Any record with data in the 2+ Removed field is excluded.

<em> Vacant Land Filter </em>: Shows all records with property type of "Vacant Land" and gisacres of <= 10 and parval of >= $100,000. Also shows records with property type of "Vacant Land" and gisacres of >= 10 and parval of >= $200,000. Any record with data in the 2+ removed field is excluded.

<em> BRE Pricing by Zone Filter </em>: Shows records which meet the following criteria. Property Type is either "Home" or "Home / Potentially Not Residential" (because we can't always distinguish in some counties). Then each zone (1a through 6a) and the price filter for each respective zone. Any record with data in the 2+ removed field is excluded. **This filter took a long time to create. Please don't delete it!**. You can modify this by changing any of the prices for each zone. 




Geography Filters

This is not the official name used by Mapsimise. I made it up. A "geographical filter" is any shape on the map used to filter the master list. There are many ways to make geographical filters. These filters are located (by default) about 2/3 of the way down the page, right above the layers and the data table. You can also double up filters. For example you can (and probably should) use a data filter and then a geographical filter on the data. 

<em> River Filters </em> I have created 3 pre-existing river filters. These are meant to approximate the "homes on the river" list. It is not quite the same as the parcels that exactly touch the boundary of the rivers as in QGIS. The three river filters are the Watauga River Filter, South Fork New River Filter, North Fork New River Filter. Their buffers can be manipulated as you like. 

<em> KML Shape Filters </em> I have pre-loaded many other shapes into the map. All the shapes have been uploaded as KML files and are readily available. First, at the top of the screen find "KMLs". The current options are: Zone Shapes (ex: Zone1a), Watauga River, New River, High Elevations. The high elevations filter are all areas in the high county above 4000ft.

To use one of these as a data filter, from the "KMLs" list, click on one and it will pop up on the right side of the screen









