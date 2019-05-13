
# README

# Changes Made Inorder to get Optimal Performance.

* Added cloud Source and enabled withCredentials to true to handle Cross Orgin Resource Sharing(CORS) on chrome.

* Made changes to Viewr.html file and set PointBudget 1.25Mn which showed optimal memory usage and maintained           consitant FPS.

* The more the pointBudget Set the higher the memory consumption.

* Changed the material size and found out the smaller the better the FPS & lesser memory.

* Changed PointShape of material to SQUARE, CIRLCE and found out both these works in a similar fashion and has doesn't make big difference in terms of performance. But, PARABOLOID has effected the memory in small amounts.

* PointSizeType of material made a small shift in memory when given ADAPTIVE but gave much better intensity & Depth;

* Modified PointColorType of material to INTENSITY and observed a slight better variation in memory compared to RGB. With RGB it gives a much better user Experience.

* Had run permutations and combinations of these properties of the potree and figured few optimal values which would be give better performance for the given pointcloud.

# Here are final Values.

```
    viewer.setPointBudget(1.25*1000*1000);

    material.pointColorType = Potree.PointColorType.RGB;
    material.shape = Potree.PointShape.SQUARE;
    material.size = 1; 
    material.pointSizeType = Potree.PointSizeType.CIRCLE;
```

# Observations & References.

## From what I have understood about how Three.JS and WEBGL works together to render massive pointclouds with the help of Potree algos, here are my takeaways and strategies to get better performance:

* Creating sub pointclouds and loading these small pointclounds on demand which would finally combined to form the main pointcloud would have massive impact on storage and memory.
* Having Elapsed requestAnimationFrame or controlling the frame rate using the execution of last frame in our draw would help us maintain consistent fps.
```
var fps = 30;
var now;
var then = Date.now();
var interval = 1000/fps;
var delta;
  
function draw() {
     
    requestAnimationFrame(draw);
     
    now = performance.now();
    delta = now - then;
     
    if (delta > interval) {
        then = now - (delta % interval);
        // We can write our code here         
    }
}
 
draw();

```
* Giving Optimal NodeSize also improves the perfomance.
* Creating seperate BufferGometry file and load it via BufferGeometryLoader, as it is the heaviest chunk of file.
* Continuos monitoring number of draw calls.
* Stopping requestAnimationFrame when the view is Static.
* Loading mutliple pointclouds over network or lazy loading based on the point user view is in would help massively with the performance.
* The above mentioned can be acheived through tiling. A process where we load certain tiles and track the moment and load the next times based on latitude & longitude.
* Loading low depth fragments initially and lazy loading them on demand would reduce initial load time.
* Continuos Dispose of the gometry you have Leat Recently used helps clear the buffer and improves memory and performance.

## Read in article.

* PotreeConverter is implemented as a single-process tool and for very large data sets its computing time is too large.
One alternative to improve the performance to add multi-processing in PotreeConverter.cont...
* reference:  https://www.researchgate.net/publication/284617106_Taming_the_beast_Free_and_open-source_massive_point_cloud_web_visualization


* [Getting Started](./docs/getting_started.md)

## About

Potree is a free open-source WebGL based point cloud renderer for large point clouds.
It is based on the [TU Wien Scanopy project](https://www.cg.tuwien.ac.at/research/projects/Scanopy/)
and it was part of the [Harvest4D Project](https://harvest4d.org/).


<a href="http://potree.org/wp/demo/" target="_blank"> ![](./docs/images/potree_screens.png) </a>

Newest information and work in progress is usually available on [twitter](https://twitter.com/m_schuetz)

Contact: Markus Schütz (mschuetz@potree.org)

Reference: [Potree: Rendering Large Point Clouds in Web Browsers](https://www.cg.tuwien.ac.at/research/publications/2016/SCHUETZ-2016-POT/SCHUETZ-2016-POT-thesis.pdf)

## Build

Make sure you have [node.js](http://nodejs.org/) installed

Install all dependencies, as specified in package.json, 
then, install the gulp build tool:

    cd <potree_directory>
    npm install --save
    npm install -g gulp
    npm install -g rollup

Use the ```gulp watch``` command to 

* create ./build/potree 
* watch for changes to the source code and automatically create a new build on change
* start a web server at localhost:1234. Go to http://localhost:1234/examples/ to test the examples.

```
gulp watch
```

## Downloads

[PotreeConverter source and Win64 binaries](https://github.com/potree/PotreeConverter/releases)

## Showcase

Take a look at the [potree showcase](http://potree.org/wp/demo/) for some live examples.

## Compatibility

| Browser              | OS      | Result        |   |
| -------------------- |:-------:|:-------------:|:-:|
| Chrome 64            | Win10   | works         |   |
| Firefox 58           | Win10   | works         |   |
| Edge                 | Win10   | not supported |   |
| Internet Explorer 11 | Win7    | not supported |   |
| Chrome               | Android | works         | Reduced functionality due to unsupported WebGL extensions |
| Opera                | Android | works         | Reduced functionality due to unsupported WebGL extensions |

## Credits

* The multi-res-octree algorithms used by this viewer were developed at the Vienna University of Technology by Michael Wimmer and Claus Scheiblauer as part of the [Scanopy Project](http://www.cg.tuwien.ac.at/research/projects/Scanopy/).
* [Three.js](https://github.com/mrdoob/three.js), the WebGL 3D rendering library on which potree is built.
* [plas.io](http://plas.io/) point cloud viewer. LAS and LAZ support have been taken from the laslaz.js implementation of plas.io. Thanks to [Uday Verma](https://twitter.com/udaykverma) and [Howard Butler](https://twitter.com/howardbutler) for this!
* [Harvest4D](https://harvest4d.org/) Potree currently runs as Master Thesis under the Harvest4D Project
* Christian Boucheny (EDL developer) and Daniel Girardeau-Montaut ([CloudCompare](http://www.danielgm.net/cc/)). The EDL shader was adapted from the CloudCompare source code!
* [Martin Isenburg](http://rapidlasso.com/), [Georepublic](http://georepublic.de/en/),
[Veesus](http://veesus.com/), [Sigeom Sa](http://www.sigeom.ch/), [SITN](http://www.ne.ch/sitn), [LBI ArchPro](http://archpro.lbg.ac.at/),  [Pix4D](http://pix4d.com/) as well as all the contributers to potree and PotreeConverter and many more for their support.
