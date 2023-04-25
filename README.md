# Control-charts-in-Power-BI
When it comes to control limits, there are custom visuals you can download from Powr BI apps or embd from open-sourced programming which is always full of graphing packages. However, you may encounter either a license issue or frigid designs that don't meet your requirements.  This repository shows you how to create your own control limits using the basic Power BI visual, line chart.
![line](https://user-images.githubusercontent.com/22305109/234286654-564179c5-ecdd-4330-8c6e-a6138f40d6a5.PNG)
# Control limits
Mathematical definitions: The central limit is the average. The upper limit is a multiple of the standard deviation above the average. The lower control limit is a multiple of the standard deviation below the average. Depending on your business needs, you can decide how many units of standard deviation in the calculation. However, the tricky part is: These statistics are calculated based off the numbers from prior time frames, like a quarter ago. 
# Data model strcuture that works in Power BI
When we use open-sourced programming such as R or python, the calculations from numbers in different dates are easily coded. However, Power BI has its own way in data wrangling from the perspective of lowe-code environment. Thus, I developed this to make our own control charts weaved in Power BI. 
## define the time frames used for calculating control limits
## create table join links
## merge tables
## plot it on line chart!
