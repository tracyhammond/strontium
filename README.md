COPYRIGHT NOTICE
----------
We are still working on the license information for this project. Most code here is released under a BSD license, copyright the [Sketch Recognition Lab](http://srlweb.cse.tamu.edu) of Texas A&M University.
Where not indicated, please assume a BSD license. If you notice any inconsistencies please let us know as we are working to clean this code up.


Strontium Library (SrL)
----------------------
The Strontium Library is a collection of sketch recognition libraries. It is split into 6 distinct pieces:

1. Core - This contains core classes for collecting and storing sketches, including Sketch, Stroke, Point and Shape.
2. Recognition - This contains several sketch recognition libraries, of particular interest is the basic shape recognizer [PaleoSketch](http://srlweb.cse.tamu.edu/srlng/research/project/1)
3. Distributed – This contains client/server code for a messaging system for performing distributed sketch recognition.
4. Legacy – This contains some legacy dependencies, not needed for any new application.
5. Swing UI - This contains a few basic Java Swing user interface classes
6. Android UI - This contains a few basic Android user interface views


Dependencies
--------------
In order to use the strontium library correctly, you must include the following libraries:

1. [OpenAWT](https://github.com/eyce9000/OpenAWT) - An implementation of java.awt.geom that runs on Android and on the JDK
2. [Jackson JSON](http://jackson.codehaus.org/) - Used for serializing sketches to JSON
3. [Simple XML](http://simple.sourceforge.net/) - Used for serializing sketches to XML
4. [Jama](http://math.nist.gov/javanumerics/jama/) - Standard matrix library
5. [Weka](http://www.cs.waikato.ac.nz/ml/weka/) - Machine learning library

Sample Applications
------------
To see what basic shape recognition can do, try out the following two applications:

[PaleoSketch Online](http://srl-mechanix.appspot.com/) is an in-browser recognizer that you can try out. It performs recognition on a Google App Engine instance and returns recognition results.

[Sketchpad](https://github.com/eyce9000/strontium/downloads) is a simple Android application that provides a drawing canvas that performs automatic basic shape recognition as you draw.

Simple Example
-------
The following example shows how to create a sketch object, add a stroke made up of points and perform basic shape recognition on it:
```````java
	/* 
	 Create the sketch. This would normally be done by collection points from user interaction
	*/
	Sketch sketch = new Sketch();
	
	Stroke stroke = new Stroke();
	for(int i=0; i<20; i++){
	  stroke.add(new Point(i,i));
	}
	
	sketch.add(stroke);	
	
	/*
	 Run basic shape recognition on the first stroke of the sketch (the one we just created)
	 This should result in a best shape label of "Line"
	*/
	PaleoSketchRecognizer recognizer = new PaleoSketchRecognizer(PaleoConfig.allOn());
	IRecognition result = recognizer.recognize(sketch.getFirstStroke());
	
	if(result.getBestShape().label.equalsLowerCase("line"))
	  System.out.println("Correctly recognized as a line");
```````

Configuring PaleoSketch
-----------------------
To instantiate the PaleoSketchRecognizer you must first create a PaleoConfig. This allows you to specify which set of shapes you would like to recognize.

`````````java
	/*
	 By default, every shape is enabled when using the plain constructor
	*/
	PaleoConfig config = new PaleoConfig();

	/*
	 Or you can pass a list of shape Options to specify which to enable. All other shapes will be disabled.
	*/
	config = new PaleoConfig(PaleoConfig.Option.Line, PaleoConfig.Option.Circle, PaleoConfig.Option.Polyline);

	/*
	 Or you can use one of several predefined recognition sets.
	*/
	config = PaleoConfig.allOn();
	config = PaleoConfig.basicPrimsOnly();
`````````

