# fl_animated_linechart

[![Codemagic build status](https://api.codemagic.io/apps/5d5e513ff8278e001ca52adf/5d5e513ff8278e001ca52ade/status_badge.svg)](https://codemagic.io/apps/5d5e513ff8278e001ca52adf/5d5e513ff8278e001ca52ade/latest_build)

![Animations](chart.gif)

An animated chart library for flutter.
 - Support for datetime axis
 - Multiple y axis, supporting different units
 - Highlight selection
 - Animation of the chart
 - Possibility of adding legends
 - Support for both horizontal and vertical markerlines 
 - Tested with more than 3000 points and still performing

There are currently two different charts:
 - line chart
 - area chart

## Getting Started

Try the sample project or include in your project.

Highlight for the line chart:
![Chart example with highlight](withSelection.png)
![Chart example with markerlines and legends](withMarkerlinesAndLegends.png)
![Chart example with horizontal and vertical markerlines along with icons on the chart and legends](withHorizontalAndVerticalMarkerlinesAndLegends.png)

Area chart:
![Area Chart example](areaChart.png)
![Area Chart example](areaChartGradient.png)

Example code:
```dart
    LineChart lineChart = LineChart.fromDateTimeMaps([line1, line2], [Colors.green, Colors.blue]);

    return Scaffold(
      appBar: AppBar(
        title: Text(widget.title),
      ),
      body: Container(
        child: Column(
            mainAxisSize: MainAxisSize.max,
            mainAxisAlignment: MainAxisAlignment.spaceBetween,
            crossAxisAlignment: CrossAxisAlignment.stretch,
            children: [
              Expanded(child: AnimatedLineChart(lineChart)),
            ]
        ),
      ),
    );
```
<br/>
Example code with horizontal markerlines and legends:

    LineChart lineChart = LineChart.fromDateTimeMaps([
        line1,
        line2,
        line3,
        line4,
        line5
      ], [
        Colors.blue,
        Colors.red,
        Colors.yellow,
        Colors.yellow,
        Colors.red
      ], [
        'C',
        'C',
        'C',
        'C',
        'C',
      ], tapTextFontWeight: FontWeight.w400);

  To define a line as a horizontal dashed markerline, first define the line as any other regular line, then set isMarkerLine to true:<br/>

    lineChart.lines[1].isMarkerLine = true;
    lineChart.lines[2].isMarkerLine = true;
    lineChart.lines[3].isMarkerLine = true;
    lineChart.lines[4].isMarkerLine = true;
      
  Or: <br/>
  ```dart
    lineChart.lines.skip(1).forEach((line) {
        line.isMarkerLine = true;
      });
  ```
  Legends: <br/>
  A Legend has the following constructor: <br/>
  ```dart
      const Legend({this.title, this.color, this.icon, this.style});
  ```
    
  You can choose to either have a short line or an Icon as the first part of the legend before the title. If no Icon is defined, then a short line will be shown. <br/>
  You can change the Color of the line and the TextStyle of the String title. <br/>
  You can add however many Legend you want. <br/>

```dart 
      return Scaffold(
      appBar: AppBar(
        title: Text(widget.title),
      ),
      body: Container(
        child: Column(
          mainAxisSize: MainAxisSize.max,
          mainAxisAlignment: MainAxisAlignment.spaceBetween,
          crossAxisAlignment: CrossAxisAlignment.stretch,
          children: [
            Expanded(
              child: AnimatedLineChart(
                lineChart,
                toolTipColor: Colors.white,
                gridColor: Colors.black54,
                textStyle: TextStyle(fontSize: 10, color: Colors.black54),
                showMarkerLines:
                    true, // If this value is not set to true, all defines lines will be filled lines and not dashed
                legends: [
                  Legend(title: 'Critical', color: Colors.red),
                  Legend(title: 'Warning', color: Colors.yellow),
                ],
              ),
            ),
          ],
        ),
      ),
    ); 
```
<br/>
Example code with horizontal and vertical markerlines along with icons on the chart and legends:

  It is possible to define a maximum of two vertical markerlines. <br/>
  The verticalMarker variable is a ``` List<DateTime> ``` and the length can be >= 2. <br/>
  If two vertical markerlines are defined, the area between the two lines will be filled with a color that can be defined with verticalMarkerColor.<br/>

  It is possible to add an Icon on the point where the vertical markerline crosses the y-axis value by defining the verticalMarkerIcon. <br/>
  The verticalMarkerIcon variable takes a ``` List<Icon> ``` and the lenght must be equal to the length of the verticalMarker variable. <br/>
  The possibility of adding a colored background to the icons have been added and can be defined with iconBackgroundColor which takes a Color. <br/>

```dart 
      return Scaffold(
      appBar: AppBar(
        title: Text(widget.title),
      ),
      body: Container(
        child: Column(
          mainAxisSize: MainAxisSize.max,
          mainAxisAlignment: MainAxisAlignment.spaceBetween,
          crossAxisAlignment: CrossAxisAlignment.stretch,
          children: [
            Expanded(
              child: AnimatedLineChart(
                lineChart,
                toolTipColor: Colors.white,
                gridColor: Colors.black54,
                textStyle: TextStyle(fontSize: 10, color: Colors.black54),
                showMarkerLines:
                    true, // If this value is not set to true, all defines lines will be filled lines and not dashed
                legends: [
                  Legend(title: 'Critical', color: Colors.red),
                  Legend(title: 'Warning', color: Colors.yellow),
                ],
                verticalMarker:[
                          DateTime.now()
                              .subtract(Duration(minutes: 40))
                              .toLocal(),
                          DateTime.now()
                              .toLocal()
                              .subtract(Duration(minutes: 30)),
                        ],
                  verticalMarkerColor: Colors.yellow,
                  verticalMarkerIcon: [
                    Icon(
                      Icons.report_problem_rounded,
                      color: Colors.yellow,
                    ),
                    Icon(
                      Icons.check_circle_rounded,
                      color: Colors.green,
                    ),
                  ],
                  iconBackgroundColor: Colors.white,
              ),
            ),
          ],
        ),
      ),
    );
```

Example code with shaded area between markerlines:
It is possible to have shaded areas between the defined markerlines. <br/>
The shaded area will be in the same color as the markerline. <br/>
It is important that the order of enums in the filledMarkerLinesValues matches the order of defined markerlines to be shown in the graph. <br/>
Enums with the value MaxMin.MAX will draw to the top if there is only one markerline defined as MAX, otherwise it will draw from i - 1 where enum values are MAX. <br/>
If enum value is MaxMin.MIN the shaded area will draw downwards to i + 1, unless the index is the last in the list, then the shaded area will draw all the way to the bottom of the graph. <br/>
```dart 
return Scaffold(
      appBar: AppBar(
        title: Text(widget.title),
      ),
      body: Container(
        child: Column(
            mainAxisSize: MainAxisSize.max,
            mainAxisAlignment: MainAxisAlignment.spaceBetween,
            crossAxisAlignment: CrossAxisAlignment.stretch,
            children: [
              Padding(
                padding: const EdgeInsets.all(8.0),
                child: Row(
                  mainAxisSize: MainAxisSize.max,
                  mainAxisAlignment: MainAxisAlignment.spaceAround,
                  children: <Widget>[
                    TextButton(
                      style: flatButtonStyle,
                      child: Text(
                        'LineChart',
                        style: TextStyle(
                            color: chartIndex == 0
                                ? Colors.black
                                : Colors.black12),
                      ),
                      onPressed: () {
                        setState(() {
                          chartIndex = 0;
                        });
                      },
                    ),
                    TextButton(
                      style: flatButtonStyle,
                      child: Text('LineChart2',
                          style: TextStyle(
                              color: chartIndex == 1
                                  ? Colors.black
                                  : Colors.black12)),
                      onPressed: () {
                        setState(() {
                          chartIndex = 1;
                        });
                      },
                    ),
                    TextButton(
                      style: flatButtonStyle,
                      child: Text('AreaChart',
                          style: TextStyle(
                              color: chartIndex == 2
                                  ? Colors.black
                                  : Colors.black12)),
                      onPressed: () {
                        setState(() {
                          chartIndex = 2;
                        });
                      },
                    ),
                    TextButton(
                      style: flatButtonStyle,
                      child: Text('MarkerLines',
                          style: TextStyle(
                              color: chartIndex == 3
                                  ? Colors.black
                                  : Colors.black12)),
                      onPressed: () {
                        setState(() {
                          chartIndex = 3;
                        });
                      },
                    ),
                  ],
                ),
              ),
              Expanded(
                  child: Padding(
                padding: const EdgeInsets.all(8.0),
                child: AnimatedLineChart(
                  chart,
                  key: UniqueKey(),
                  gridColor: Colors.black54,
                  textStyle: TextStyle(fontSize: 10, color: Colors.black54),
                  toolTipColor: Colors.white,
                  legends: chartIndex == 3
                      ? [
                          Legend(title: 'Critical', color: Colors.red),
                          Legend(title: 'Warning', color: Colors.yellow),
                        ]
                      : null,
                  showMarkerLines: chartIndex == 3 ? true : false,
                  fillMarkerLines: chartIndex == 3 ? true : false,
                  verticalMarkerColor: chartIndex == 3 ? Colors.yellow : null,
                  verticalMarkerIcon: [
                    Icon(
                      Icons.report_problem_rounded,
                      color: Colors.yellow,
                    ),
                    Icon(
                      Icons.check_circle_rounded,
                      color: Colors.green,
                    ),
                  ],
                  iconBackgroundColor: Colors.white,
                  filledMarkerLinesValues: chartIndex == 3
                      ? [
                          MaxMin.MAX,
                          MaxMin.MAX,
                          MaxMin.MIN,
                          MaxMin.MIN,
                        ]
                      : [],
                ), //Unique key to force animations
              )),
              SizedBox(width: 200, height: 50, child: Text('')),
            ]),
      ),
    );
```

The example app, can toggle between line chart and area chart.
![Example app](exampleScreenshot.png)
