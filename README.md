Highcharts Serverside Export
============================

This is a fork of the Highcharts Serverside Export project from One2Team.

The original project can be found here:

https://github.com/one2team/highcharts-serverside-export

I forked this project as I wanted to move the build system outside of
Eclipse. So, I've enabled this project for Maven - it should build
out of the box with the following command:

> $ mvn clean package

However, I've found that running Rhino+batik on the server-side has not 
been very performant. The Highcharts team reached the same conclusion, 
and as a result, they took a different approach to server-side rendering of 
charts; they have built a solution that requires phantomjs and a standalone
servlet container. This solution works well - if you have no restrictions
on what you can deploy to your environment I would strongly encourage
you to use it:

https://github.com/highcharts/highcharts-export-server

However, if you are limited in what you can deploy to your environment, 
or want to avoid phantomjs for some reason, then this project might be of
use.

The original notes from One2Team continue below.

--------------------------------------------


The primary goal of the Highcharts Serverside Export framework (HSE) is to provide a java API for Highcharts including image generation capabilities.

Solution features :

* Java API corresponding to [Highcharts](http://www.highcharts.com) model (kept compatible with Highcharts model hierarchy and properties),
* Rhino-Apache Batik based renderer : java ChartOptions ==> Rhino ==> Highcharts ==> SVG ==> image (png, JPEG, etc...),



Usage
-----
You can use the framework to generate in java an image file out of a Highcharts chart.

* With Highcharts, you would generate a javascript chartOptions, which is shown in browser
* Here, you generate a java chartOptions (same hierarchy, same properties), which is converted in image stream (using Rhino, Highcharts and Batik) 


Getting Help
------------
To start to use it, you need to know Highcharts API.

## Highcharts
* See [Highcharts](http://www.highcharts.com/)
* See [Highcharts API Reference](http://www.highcharts.com/ref/)

## Highcharts Serverside Export java doc
* See embeded java doc (./doc)

## Rhino
* See [Rhino](http://www.mozilla.org/rhino/)

## Batik SVG Toolkit
* See [Apache Batik SVG Toolkit](http://xmlgraphics.apache.org/batik/)

Quick Start
-----------
## A java sample : SimpleExport (test/examples/SimpleExport.java)

	public static void main (String[] args) {
		
		// This executable expects an export directory as input
		File exportDirectory = new File (args [0]);
		
		// ====================================================================
		// ChartOptions creation
		// ----------------------
		//  The createHighchartsDemoColumnBasic method describes the creation of 
		//   a java chartOption. It is a java equivalent to javascript Highcharts sample
		//   (see http://highcharts.com/demo/column-basic)
		ChartOptions chartOptions1 = SamplesFactory.getSingleton ().createColumnBasic ();

		// ====================================================================
		// Chart export
		// ----------------
		// Inputs :
		//    1. chartOptions : the java ChartOptions to be exported,
		//    2. exportFile  : file to export to.
		HighchartsExporter pngExporter = ExportType.png.createExporter ();
		pngExporter.export (chartOptions1, null, new File (exportDirectory, "column-basic.png"));
		...
  }

* export directory
You can choose the export directory by changing the java line :
  * windows :  File exportDirectory = new File ("D:\\");
  * linux : File exportDirectory = new File ("/home/myself/export-dir");
  * ...
  
* SimpleExample shows several cases of charts export :
  * from 'java' chartOptions (column-basic) to png file,
  * from 'java' chartOptions (pie-chart) to png file,
  * from 'java' chartOptions (time-data-with-irregular-intervals) to jpeg file,
  * from 'json' chartOptions (column-basic) to png file,
  * from 'java' chartOptions (column-basic) to json chartOptions (as a String).
  
* Please note that following trace do not mean a mistake :
log4j:WARN No appenders could be found for logger (org.one2team.highcharts.server.export.util.SVGHighchartsHelper).
log4j:WARN Please initialize the log4j system properly.
ERROR: The JPEG quality has not been specified. Use the default one: no compression 
  
SamplesFactory contains three methods that generate equivalent of Highcharts demo gallery examples :
* SamplesFactory.createColumnBasic : [Basic column](http://highcharts.com/demo/column-basic)
* SamplesFactory.createPieChart : [Pie chart](http://highcharts.com/demo/pie-basic)
* SamplesFactory.createTimeDataWithIrregularIntervals : [Time data with irregular intervals](http://highcharts.com/demo/spline-irregular-time)

## Running the sample
  
* jars
All the dependencies files are provided in ./lib/

* Eclipse :
  The framework is eclipse ready.
   * open the project in eclipse,
   * run SimpleExport
   
* other IDE
   * open the project in your favorite IDE,
   * declare all the ./lib jars as dependencies in your project,
   * run SimpleExport
   
   
Copyright and License
---------------------

Licensed under the Apache License, Version 2.0 (the "License"); you may not use this work except in compliance with the License. You may obtain a copy of the License in the LICENSE file, or at:

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.
