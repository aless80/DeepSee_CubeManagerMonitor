# DeepSee_TimeCharts
A DeepSee portlet to chart dates and times using [Amcharts](https://www.amcharts.com/)

### Description
This project shows a portlet using 3rd party tools for charting date and time.
The files contain a Patients2 cube. The Patients2 cube is identical to the Patients cube in SAMPLES but with an additional BirthDateTime dimension that is displayed as the x-axis for the portlet. 
See the documentation on portlets: [Creating Portlets for Use in Dashboards](http://docs.intersystems.com/latest/csp/docbook/DocBook.UI.Page.cls?KEY=D2IMP_ch_portlets).

### Content
Patients2 cube and a portlet showing a chart based on Amcharts. The x-axis of the chart is based on date-time (eg "2017-11-12 09:18:00") and it also works with time dimensions based on date, months (less nice).

![Alt Text](https://github.com/aless80/DeepSee_TimeCharts/blob/master/img/TimeAmchart.png)           


### Instructions
#### Programmatic import from Caché console
```
ZN "SAMPLES"
Set path="/home/amarin/DeepSee_TimeCharts/"  //Set your path
W $system.OBJ.Load(path_"DeepSee.Model.PatientsCube2.cls","cf")  //import the Patients2 cube
W ##class(%DeepSee.Utils).%BuildCube("Patients2",1,1)
W ##class(%DeepSee.TermList).%ImportCSV(path_"PATIENTS COLSPECS.txt") //termlist
W $system.OBJ.Load(path_"Ale.PortletAmcharts.cls","cf")
W $system.OBJ.Load(path_"Ale.PortletAmchartsREST.cls","cf")
Do ##class(%DeepSee.UserLibrary.Utils).%Import(path_"Patients2.pivot.DFI",1)
Do ##class(%DeepSee.UserLibrary.Utils).%Import(path_"PortletAmcharts.dashboard.DFI",1)
Do ##class(%DeepSee.UserLibrary.Utils).%Import(path_"PortletAmchartsREST.dashboard.DFI",1)
```

If your instance does not support UDL formatting please use the .xml files in the xml directory.

#### Manual import
1) In the SAMPLES namespace import the Patients2 cube in PatientsCube2.xml. This file contains the cube class for Patients2;
2) Build the cube:
```
W ##class(%DeepSee.Utils).%BuildCube("Patients2",1,1)
```
3) Import the portlet class Ale.PortletAmchartsREST.xml in studio;
4) Import the portlet class Ale.PortletAmcharts.xml in studio;
5) Import the pivot Patients2.pivot.DFI;
6) Import the termlist PATIENTS COLSPECS.txt to be able to use the Choose Column Spec control on the dashboard;
7) Open the PortletAmcharts and PortletAmchartsREST dashboards.


### Limitations
The default filter control is not used when you first load the dashboard. This has been ProdLogged.  
The current implementation of onApplyFilters calls renderContents. This makes the filters work but renderContents runs two times at startup. This has been ProdLogged.  
The data should be sorted by date and be in the format "yyyy-mm-dd hh:mm:ss" eg "2018-07-27 14:25:41".  
The pivot should not have crossjoins  
Only one chart/column is plotted  
Buy a license from AmCharts to get rid of their tag on the top left of the graph
