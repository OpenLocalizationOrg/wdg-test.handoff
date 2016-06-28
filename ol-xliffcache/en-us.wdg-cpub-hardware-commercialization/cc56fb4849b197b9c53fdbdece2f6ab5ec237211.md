---
title: Graphs
description: Graphs
MSHAttr:
- 'PreferredSiteName:MSDN'
- 'PreferredLib:/library/windows/hardware'
ms.assetid: 44ccfef7-aaee-4194-a26d-5285e41eb683
ms.prod: W10
ms.mktglfcycl: operate
ms.sitesec: msdn
---

# Graphs


Windows Performance Analyzer (WPA) provides the following types of graphs:

-   Line graphs

-   Stacked-line graphs

-   Stacked-bar graphs

-   Lifetime graphs

-   Activity type graphs

-   Sequential resource trails

-   Generic events graphs

## Line, Stacked-Line, and Stacked-Bar Graphs


When you drag a line, stacked-line, or stacked-bar graph from the **Graph Explorer** window to the **Analysis** tab, it appears as a line graph, as shown in the following illustration

![line graph](images/wpaline.jpg)

To change the appearance of the graph, click the right-most drop-down arrow on the graph title bar and select either **Stacked Lines** or **Stacked Bars**.

The following illustration shows the same graph as a stacked-line graph.

![stacked line graph](images/wpastackedline.jpg)

The following illustration shows the same graph as a stacked-bar graph.

![stacked bar graph](images/wpastackedbar.jpg)

You can specify from 4 to 100 intervals for a stacked-bar graph.

## Lifetime Graphs


Lifetime graphs show individual categories, such as processes, as horizontal bars that define the lifetime of the category.

The following illustration shows a process lifetime graph.

![process lifetime graph](images/processlifetime.jpg)

## Activity Type Graphs


Activity type graphs use horizontal bars to show types of activity. On each bar, the time of the activity is shaded. If you zoom in to sufficient detail on this type of graph, you can see, for example, how long each disk read or write took.

The following illustration shows input and output by type.

![wpa i/o by process](images/wpaio.jpg)

## Generic Events Graphs


Generic event graphs show all Crimson events in the recording.

The following illustration is an example of a generic events graph.

![wpa generic events graph](images/wpagenericevents.jpg)

## Related topics


[WPA Features](wpa-features.md)

 

 







