# DeepSee_CubeManagerMonitor
DeepSee dashboard base on portlets and [Amcharts](https://www.amcharts.com/) to analyze the data in the Cube Manager


### Description
This project shows a dashboard that can be used to monitor the DeepSee [Cube Manager](https://docs.intersystems.com/latest/csp/docbook/DocBook.UI.Page.cls?KEY=D2IMP_ch_current#D2IMP_current_cubemgr). The dashboards is based on the cube CubeManagerCube and contains two widgets. One widget shows the latest synch or build events with the corresponding number of errors. The other widget appears when two particular filters are selected and shows a time chart. The time chart can be used to monitor performance, errors, and more through time. 

### Content
The CubeManagerCube cube, a dashboard (Cube Manager Monitor) with a portlet-based widget (Times by StartTime) and a scorecard widget (Latest Events By CubeKey), the CubeManagerPortlet portlet, the MaxFieldPlugin.xml KPI plugin for the scorecard widget, the CubeManager_colspec.txt termlist for the portlet-based widget. 

The portlet shows a chart based on Amcharts, which is an updated version of the AmCharts-based portlet available on my repository [DeepSee_TimeCharts](https://github.com/aless80/DeepSee_TimeCharts). The x-axis of the chart is based on date-time (e.g. "2018-08-02 14:18:00"). The portlet allows users to set several settings from the dashboard, including a list of required filters (an option not yet available in DeepSee!). See the documentation on portlets: [Creating Portlets for Use in Dashboards](http://docs.intersystems.com/latest/csp/docbook/DocBook.UI.Page.cls?KEY=D2IMP_ch_portlets).

The scorecard widget is based on a pivot using a KPI plugin. The KPI plugin allows users to return the field of a source records. The source record selected is %CONTEXT dependent and is the one where another field is biggest. This is used to get the date/errors of the latest synch/build event (i.e. the date/errors of the fact with the biggest timestamp). 

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
This routine is **not officially supported by InterSystems Co.** I suggest using this routine only in test environments.
The default filter control in the portlet widget is not used when you first load the dashboard. This has been ProdLogged.  
The current implementation of onApplyFilters calls renderContents. This makes the filters work but renderContents runs two times at startup. This has been ProdLogged.  
The data should be sorted by date and be in the format "yyyy-mm-dd hh:mm:ss" e.g. "2018-07-27 14:25:41".  
The pivot should not have crossjoins  
Buy a license from AmCharts to get rid of their tag on the top left of the graph
