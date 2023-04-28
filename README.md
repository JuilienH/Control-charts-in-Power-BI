# Control-charts-in-Power-BI
When it comes to control limits, there are custom visuals you can download from Powr BI apps or embd from open-sourced programming which is always full of graphing packages. However, you may encounter either a license issue or frigid designs that don't meet your requirements.  This repository shows you how to create your own control limits using the basic Power BI visual, line chart.
![line](https://user-images.githubusercontent.com/22305109/234286654-564179c5-ecdd-4330-8c6e-a6138f40d6a5.PNG)
# Control limits
Mathematical definitions: The central limit (CL) is the average. The upper control limit (UCL) is a multiple of the standard deviation above the average. The lower control limit (LCL) is a multiple of the standard deviation below the average. Depending on your business needs, you can decide how many units of standard deviation in the calculation. However, the tricky part is: These statistics are calculated based off the numbers from prior time frames, like a quarter ago. 
# Data model strcuture that works in Power BI
When we use open-sourced programming such as R or python, the calculations from numbers in different dates are easily coded. However, Power BI has its own way in data wrangling in the low-code environment. Thus, I developed this to make our own control charts weaved in Power BI data structure. 
## define the time frames used for calculating control limits
We want to compare how we currently perform to the prior quarter, so I wrote the queries in which the time frame is defined. When importing data, you can put the time statement in WHERE clause. Here is my example when I query against Microsoft SQL Server Analysis Services:
![import](https://user-images.githubusercontent.com/22305109/234293734-836685b2-5908-4ab1-a6bf-2bf912448fcc.PNG)

## create table join links
This step is crucial to associate the calculations with the data at a different point of time. This concept is to create a table-join key, composed of the date difference information. In the tables where I calculated control limits, I would put 20222 for 2022 Q1 data, as an example. And then the main table's table-join key will include the actual date information, derived from the Date column. I created the custom column for the table-join keys in Power Query Editor:

![custom](https://user-images.githubusercontent.com/22305109/234354347-c016bef5-a47c-4f5f-a333-aab001e43463.PNG)

Here is the DAX for getting the year and quarter number from Date column:
```
TableJoinKey=Text.From(Date.Year([#"[Date]"])) & Text.From(Date.QuarterOfYear([#"[Date]"]))
```

## merge tables
In Power Query Editor, I then was able to manually merge tables to bring in control limits from the prior quarter for each quarter.
![merge](https://user-images.githubusercontent.com/22305109/234357040-3d8b2fa1-8c8b-44dd-82bc-b2aa1fb08f63.PNG)

## plot it on line chart!
Once the merge table is created, I have all I have to plot a control chart using Line Chart visual in Power BI within few clicks! Simply put the date on the X-axis and any daily measures on the Y-axis as well as the control limits. As you can see as below, the control limits are nicely positioned at each quarter. You can see the line values shift when the quarter transits. 

![chart](https://user-images.githubusercontent.com/22305109/234358302-5aa3d9a7-fcf3-478c-b4d4-d812ae264a04.PNG)

## Rolling averages
There is a quick measure you can tap in Power BI to get the rolling average for a numeric field. 

![r](https://user-images.githubusercontent.com/22305109/235180835-6cd6c015-83bb-45d9-a05d-aed9f92e7df2.PNG)

If it doesn't work for your visual, here is the DAX I successfully used to overlay on the control charts.

```
rolling_average_measure = 
VAR __LAST_DATE = LASTDATE('table'[Date])
VAR MyIndex='table'[Index]
VAR myresult=
    AVERAGEX(
        FILTER(
            'table',
            'table'[Index]> MyIndex-7 &&
            'table'[Index] <=MyIndex
        ),'table'[numeric_field]
    )
RETURN IF(__LAST_DATE > TODAY(), BLANK(), FIXED(myresult,2))
```
