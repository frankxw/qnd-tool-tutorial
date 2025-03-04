# qnd-tool-tutorial
Quick n Dirty Math Tool Tutorial

## Foreword
The tool in this project is really simple, just open the [html](./sim.html) page in a web browser!  Step through the branches to see it evolve.  Remember that this is supposed to be quick and dirty, so convenience is prioritized over robustness.

## Part 1
We have a very bare bones web page so far.  One of the most convenient tools I can recommend is [jQuery](https://jquery.com/).  While you can make a dynamic page in vanilla JS, it's much quicker to use jQuery.  We will mostly be using it for DOM manipulation (ex: when a button is clicked, we will run a function and generate/display results).  The simplest way to include jQuery is a script tag in the header.  Head on over to the offical [CDN](https://releases.jquery.com/) and click on `minified` for the proper script tag.  On the [sim.html](./sim.html) there's a simple example of a button that when clicked will generate new page elements.  Note: our "main" method will be reacting to the page loading with the following:
```
$(document).ready(function() {
    // do stuff
});
```

The main thing we want to do right now is react to button clicks.  To do so we will use the following:
```
$("#test-button").click(function() {
    makeResults();
});
```
The `$("selector")` is the shortcut syntax that jQuery provides to grab a DOM element on the page.  For our selector we use `#` to select an element by `id` and then fill in the name we used in the HTML: "test-reesults" (from the element `<div id="test-results">`).  Note: you can select by class with `.`, so if you had something like `<div class="my-class">` you would use `$(".my-class")`.

Finally, to generate stuff on the page, we are using:
```
function makeResults()
{
    $("#test-results").html("Whoa, it did <i>something</i>?");
    $("#results-area").show();
}
```
`element.html("html goes here")` is the jQuery method to replace the inner html of the specified element.

## Part 2
Now we will start doing something useful.  Let's imagine we want a tool to spit out some numbers and base those numbers on a [bezier curve](https://en.wikipedia.org/wiki/B%C3%A9zier_curve).  The cubic bezier function expects 4 control points (where the first and last are the start and end points of the curve):
```
B(t) = (1−t)^3*P0 + 3*(1−t)^2*t*P1 + 3*(1−t)*t^2*P2 + t^3*P3 where 0 ≤ t ≤ 1
```
We are going to calculate [this curve](https://www.desmos.com/calculator/yxgjpzx162).  We setup our control points as follows:
```
var P0 = {x:0, y:0};
var P1 = {x:3, y:9};
var P2 = {x:6, y:10};
var P3 = {x:10, y:10};
```
We are setting these up as Javascript objects - you can think of these like dictionaries in other languages.

Now all we have to do is calculate the x,y values at certain points and spit out the results in a table.  We'll make the assumption that we want to output 10 evenly spaced slices of the curve (technically 11).  See `calculatePointOnCurve()` and note that this is quick and DIRTY.

Side note, you may have noticed the following:
```
if(t === 0) {
    return P0;
}
```
Do yourself a favor and just default to triple equals `===` in Javascript.  You'll save yourself lots of headaches if you use [strict equals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Strict_equality) by default - avoiding weird conversions (unless you intend to do so, in which case use `==`).
