# flowSVG
Create simple interactive and static flow charts using JavaScript and SVG. 

[See an example](http://rtalbot89.github.io/flow-svg/). The code for this is in example/.

flowSVG depends on [svg.js](http://svgjs.com/). Optionally, for smooth scrolling to off-screen elements it uses jQuery and 
the [jquery.scrollTo plugin](https://github.com/flesler/jquery.scrollTo)
## Usage
### Setup
Link to the dependencies in the head of the page.

```html
<!-- jQuery only required for smooth scrolling -->
<script src="jquery-2.1.3.min.js"></script>
<script src="jquery.scrollTo.min.js"></script>
<script src="svg.js"></script>
<script src="flowsvg.min.js"></script>
```
### Quick start
Start by attaching an SVG element to a div.

```html
<div id="drawing" style="margin-left:10px"></div>
```
```javascript
<script>
flowSVG.draw(SVG('drawing').size(900, 1100));

// Then construct an array of shape objects.
flowSVG.shapes([
  {
  label: 'knowPolicy',
  type: 'decision',
  text: [
  'Do you know the ',
  'Open Access policy',
  'of the journal?'
  ],
  yes: 'hasOAPolicy',
  no: 'checkPolicy'
  }, 
  {
  label: 'hasOAPolicy',
  type: 'decision',
  text: [
  'Does it have Open',
  'Access paid option or is it an',
  'Open Access journal?'
  ],
  yes: 'CCOffered',
  no: 'canWrap'
  }
  // more shapes...
]);
</script>
```
The first shape in the array must be the starting shape of the chart. It must be a decision shape. After that, the order is irrelevant.
Each shape object has the following required properties
```javascript
 {
  // The label is how the shape is identified
  label: 'knowPolicy',
  // type may be decision, finish, or process
  type: 'decision',
  /* 
  text is an array of lines. SVG text doesn't
  wrap, so you have to manually adjust line length
  */
  text: [
  'Do you know the ',
  'Open Access policy',
  'of the journal?'
  ],
  /* 
  Where to go from here. 'yes' and 'no' apply to
  decisions. 'next' is used with processes.
  These don't apply to finish shapes for obvious
  reasons.
  */
  yes: 'hasOAPolicy',
  no: 'checkPolicy'
  // If this was a process...
  // next: ...
  }, 
```
## Options
### Orientating shapes
By default 'yes' choices are placed below the referring shape, 'no' choices to the right, and 'next' below. 

To change any of these add an 'orient' object to the shape. valid choices are 'r' (right) and 'b' (bottom).
```javascript
orient: {
  yes:'r',
  no: 'b'
  // If there is a next...
  //next: 'r'
}
```
To move a shape more to the right than the default in the static view add a 'moveRight' property to it.
```javascript
moveRight: 250
```
### Appearance
Use the config function to change the default properties e.g.
```javascript
flowSVG.config({
    // Shape width
    w: 200,
    // Shape height
    h: 180,
    // The following are self-explanatory
    connectorLength: 100,
    connectorStrokeWidth: 3,
    arrowColour: 'lightgrey',
    decisionFill: 'firebrick',
    processFill: 'navajowhite',
    finishFill: 'seagreen',
    defaultFontSize: '14'
    // Any other configurations
});
```
To see all configuration options look at the init() function at the top of the uncompressed source file. Note that some of these are still experimental and may not work as expected, or at all.

### Change Yes and No labels
These can be changed, but only globally. For example
```javascript
flowSVG.config({
    yesLabel: 'True',
    noLabel: 'false'
});
```
Having done this you may need to increase the size of the line labels, and maybe nudge them to the right or bottom. 
Don't forget that you also have connectorLength to play with.
```javascript
flowSVG.config({
    yesLabel: 'True'
    noLabel: 'false',
    labelWidth: 40,
    labelNudgeRight: 10,
    labelNudgeBottom: 10
});
```
Note that on the static view, if there are any angle lines coming out of the right of shapes, larger labels may
overlap the elbow.

### Add links
Links can be added to decision and process shapes. Links will go after the text.
```javascript
/* Links is an array of objects */
links: [
  {
  text: 'SHERPA FACT/ROMEO ', 
  url: 'http://www.sherpa.ac.uk/romeo/index.php',
  // Optional target
  target: '_blank'
  }
],
```

### Add a tip
A pop-up tip can be added to a finish shape.
```javascript
tip: {
  title: 'HEFCE Note',
  text:[
  'You must put your',
  'accepted version into',
  'WRAP and/or subject',
  'repository within 3 months',
  'of acceptance.'
  ]
}
```

### General options
Other configuration options that can be set are:
```javascript
flowSVG.config({
  /*
  By default the chart loads in interactive mode.
  Change to false to load the static view first
  */
  interactive: false,
  /* 
  Buttons are shown by default. Set to false to
  hide them
  */
  showButtons: false,
  // Disable scrolling to off screen elements
  scrollto: true
});
```
## Building charts incrementally
Flow charts take a lot of thinking about. You can build incrementally, rather than having the whole plan before you start. Referring to a non-existent shape in the 'yes, no' or 'next' properties will cause an error. To work around this, comment out these properties or leave them out until the referenced shapes exist. 

Shapes are moved into position when another shape refers to them. So until a shape is referred to, it will pile up in the default top left position. To be able to see if a shape is hiding another shape, add the 'maxOpacity' property temporarily to the config function something like this.
```javascript
maxOpacity: 0.5
```







