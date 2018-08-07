# DeepSee_CubeManagerMonitor
DeepSee dashboard base on portlets and [Amcharts](https://www.amcharts.com/) to analyze the data in the Cube Manager


### Description
This project shows a dashboard that can be used to monitor the DeepSee [Cube Manager](https://docs.intersystems.com/latest/csp/docbook/DocBook.UI.Page.cls?KEY=D2IMP_ch_current#D2IMP_current_cubemgr). The dashboards is based on the cube CubeManagerCube and contains an updated version of the AmCharts-based portlet available on my repository [DeepSee_TimeCharts](https://github.com/aless80/DeepSee_TimeCharts).  
See the documentation on portlets: [Creating Portlets for Use in Dashboards](http://docs.intersystems.com/latest/csp/docbook/DocBook.UI.Page.cls?KEY=D2IMP_ch_portlets).

### Content
CubeManagerCube cube, a CubeManagerPortlet portlet showing a chart based on Amcharts. The x-axis of the chart is based on date-time (e.g. "2018-08-02 14:18:00").

![Alt Text](https://github.com/aless80/DeepSee_CubeManagerMonitor/blob/master/img/dashboard.png)


### Instructions
#### Programmatic import from Caché console


```
ZN "SAMPLES"
Set path="/home/amarin/DeepSee_CubeManagerMonitor/"  //Set your path
W $system.OBJ.Load(path_"CubeManagerCube.xml","cf")  //import the CubeManagerCube cube
W ##class(%DeepSee.Utils).%BuildCube("CubeManagerCube",1,1)
W ##class(%DeepSee.TermList).%ImportCSV(path_"CubeManager_colspec.txt") //termlist
W $system.OBJ.Load(path_"CubeManagerCube.xml","cf")
W $system.OBJ.Load(path_"CubeManagerPortlet.xml","cf")
W $system.OBJ.Load(path_"MaxFieldPlugin.xml","cf")
Do ##class(%DeepSee.UserLibrary.Utils).%Import(path_"Cube_Manager-Times_by_StartTime-pivot.xml",1)
Do ##class(%DeepSee.UserLibrary.Utils).%Import(path_"Cube_Manager-Latest_Events_By_CubeKey-pivot.xml",1)
Do ##class(%DeepSee.UserLibrary.Utils).%Import(path_"Cube_Manager-Cube_Manager_Monitor-dashboard.xml",1)
```

<!--If your instance does not support UDL formatting please use the .xml files in the xml directory.
-->

#### Manual import
1) In the namespace where Cube Manager runs import the CubeManagerCube cube in CubeManagerCube.xml. This file contains the cube class for CubeManagerCube;
2) Build the CubeManagerCube cube from Architect:
3) Import the portlet class CubeManagerPortlet.xml;
4) Import the plugin class MaxFieldPlugin.xml;
5) Import the dashboard Cube_Manager-Cube_Manager_Monitor-dashboard.xml;
6) Import the pivot Cube_Manager-Times_by_StartTime-pivot.xml;
7) Import the pivot Cube_Manager-Latest_Events_By_CubeKey-pivot.xml;
8) Import the termlist CubeManager_colspec.txt to be able to use the Choose Column Spec control on the dashboard;
9) Open the Cube_Manager-Cube_Manager_Monitor dashboard.


### Limitations
The default filter control in the portlet widget is not used when you first load the dashboard. This has been ProdLogged.  
The current implementation of onApplyFilters calls renderContents. This makes the filters work but renderContents runs two times at startup. This has been ProdLogged.  
The data should be sorted by date and be in the format "yyyy-mm-dd hh:mm:ss" e.g. "2018-07-27 14:25:41".  
The pivot should not have crossjoins  
Buy a license from AmCharts to get rid of their tag on the top left of the graph
