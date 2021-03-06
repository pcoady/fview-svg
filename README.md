# fview-svg
## Introduction
Encapsulate SVG in Surface, a plugin for [famous-views], the [Meteor] bridge to [famo.us].

![fview-svg](https://raw.githubusercontent.com/PEM--/fview-svg/master/assets/fview-svg-demo.gif)

:warning: Work in progress :warning:

This plugin uses SVG as [Meteor] templates. By parsing the SVG, it encapsulates the part of the SVG that you want to animate into [famo.us]  `StateModifier` and `Surfaces`. This delivers hardware accelerated performance to SVG on all browsers that supports [famo.us] and remove some .

> For SVG hardware acceleration issues, see Sara Soueidan's slides in the reference section. She provides a nice recap as well as a very nice introduction to SVG.

**demo**: [fview-svg](http://fview-svg.meteor.com/).

## Usage
Starts with the usual and add some packages:
```bash
meteor create mysvg
cd mydevices
mkdir client
meteor add gadicohen:famous-views pierreeric:fview-svg
# From here you can choose your favorite famo.us provider.
# Mine is raix:famono but it works equally fine with mjn:famous.
meteor add raix:famono
```

> Note that [raix:famono](https://atmospherejs.com/raix/famono) is not only a [famo.us] provider. You can use it to import Bower packages, raw Github repos, CDN libraries and local libraries. Putting D3.js, Sortable, Velocity, jQueryUI... in [Meteor] with it, is a no brainer. A must :star:

You can choose to write your HTML and SVG templates with Spacebar or with [Jade].
```bash
meteor add mquandalle:jade
```

For your logic, you can write yours in vanilla JS or in [CoffeeScript](https://atmospherejs.com/meteor/coffeescript):
```bash
meteor add coffeescript
```

## Preparing your assets
### Aspect ratio
When creating the template that will contain your SVG, you need to:
* Remove any width or height.
* Specify the viewBox with its value X and Y set to 0.
* Ensure the preservation of the aspect ratio using `preserveAspectRatio='xMinYMin meet'`.

**Example in Jade**
```jade
svg(viewBox='0 0 400 300', preserveAspectRatio='xMinYMin meet')
```

### SVG transformations
You should better stay off SVG transformations (translate, rotate, ...). They are not hardware accelerated, at least on Chrome. See references hereafter for detailed solutions on how to remove SVG transformations when using [Inkscape].

### Create an SVG template
There are multiple ways of using SVG inside HTML, as objects, imgs, fonts and tags. The tag based SVG directly within your HTML delivers the most efficient integration. This is what is used by **fview-svg** to bridge the SVG world to the ones of [Meteor] and [famo.us].

Once you have edited your SVG, wether it be with Sketch, Adobe Illustrator, [Inkscape] or others, I strongly advise you to remove the unnecessary bloat that these software are adding to your file. For this, I use [SVGO]. See references for details.
```bash
svgo mysvg.svg
```

Always check the optimized SVG file for ensuring that the optimization process has not deteriorated your drawings. This is simply done using your browser of choice and opening the optimized SVG file.

It is now time to import your optimized SVG file as a [Meteor] template. [Meteor] provides multiple template system. As stated before, my prefered one is [Jade]. SVG is a tag based language just like HTML is. The first step is to transform your SVG into a jade file.
```bash
html2jade mysvg.svg
```

This step should produce a `mysvg.jade` file.

> This step could be avoided and you could absolutely use HTML and Blase, the [Meteor]'s default templating system. It's a personal choice. Mine is to favor a language with stronger semantic and less verbosity. It helps me focus on what needs to be changed when editing SVG manually.

Now we will promote this jade file as a real [Meteor] template file. Basically, we will give it a name as the filename has no other meaning than storage in [Meteor]: it will be part of the single minified JS file that will get transmitted to your clients.


TODO: Exporting the attributes

## API
TODO


## Advantages
* Better minification: Using Spacebar, SVG are transformed into HTML.js,
parts of Blaze. On the wire or on the air, the SVG consumes ~20% less bandwidth.
* Your SVG lives in JS space. They can be cloned easily.
* As they are transported with your single minified JS, [Meteor]'s default behavior, there is no additional round trip time for getting your SVG.
* Your SVG are instantaneously available when your application starts.
* Insertion of your SVG in the DOM is achieved via DOM range when assets are changed, as a default [Meteor] behavior.

## History
* 0.1.1-3 - Allow simple import of SVG without shape extractions.
* 0.1.0 - Initial release.

## References
* [Remove all transforms in SVG](http://stackoverflow.com/questions/13329125/removing-transforms-in-svg-files): A reminder on how to remove unnecessary tranforms in SVG with Inkscape.
* [SVGO]: A must for optimizing SVG.
* [Html2Jade](http://html2jade.org/): A handy tool available as CLI or as a web app.
* [Viewport tutorial](http://tutorials.jenkov.com/svg/svg-viewport-view-box.html): A very detailed tutorial on most SVG aspects.
* [Sara Soueidan's slides at CSSConf EU](http://slides.com/sarasoueidan/styling-animating-svgs-with-css#/27): A must watch :star:.

[famous-views]: http://famous-views.meteor.com/ "famous-views"
[Meteor]: https://www.meteor.com/ "Meteor"
[famo.us]: http://famo.us/ "famo.us"
[SVGO]: https://github.com/svg/svgo "SVGO"
[Inkscape]: http://inkscape.org/ "Inkscape"
[Jade]: https://github.com/mquandalle/meteor-jade "Maxime Quandalle's Jade"
