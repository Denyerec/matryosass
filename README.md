# Matryosass

Derived from 'Matryoshka' (a Russian nesting doll), Matryosass is a micro Sass library that gives you a fractional, nestable, responsive, reversible, <em>n-</em>col layout system - with custom namespaced media queries. It's a lovechild of [inuit.css](http://github.com/csswizardry/inuit.css), [Don't Overthink It Grids](http://css-tricks.com/dont-overthink-it-grids), [Naming Media Queries](http://css-tricks.com/naming-media-queries/), and Bootstrap.

You tell Matryosass how many columns you want, how big of a gutter, and what breakpoints - if any - you need.  

Matryosass Grid works in fractions; no more using 'sixcol' or 'col-6' when you just mean one half. The idea being you give a column a default width value, and then use your custom named media queries to act as width modifiers. 

So if you wanted a three quarter width column you'd use `<div class="col-3-4">` and then if you wanted that to go down to 50% for medium screens and full width for very small screens, you'd use `<div class="col-3-4 md-1-2 xs-1-1">`.   


## Demo

* [View demo](http://quagliero.github.io/matryosass) (the pink is nested, the grey is reversed!)
* [View demo code](https://github.com/quagliero/matryosass/tree/gh-pages)

_Matryosass is for IE8 and above._

## Implementation

Getting a Matryosass Grid up and running is quick and easy. Download the Sass file, define your variables, `@import` it into your stylesheet and off you go. 

```scss
/* grid.scss */
$mg-fractions  : 12;
$mg-gutter     : 24px;
```
```scss
/* your .scss file */
[Your own CSS]

@import "path/to/matryosass-grid";

[More of your own CSS]

```

## Settings

### $mg-fractions
Straight out of the box you get 12 fractional widths to use. Whole, halves, thirds, quarters, fifths, sixths, sevenths... you get the idea. But you can have as many (or few) as you want. Whatever number you assign to `$mg-fractions` will be the amount you get.
```scss
// @int
$mg-fractions: 12;
```
<hr>
### $mg-gutter
Nothing crazy here, just the distance between each column. I'd recommend matching it to your base line height for some horizontal and vertical rhythm. Vertical rhythm? There are things like <a href="http://github.com/csswizardry/typecsset">Typcsset</a> which are great at this.
```scss
// @int + unit
$mg-gutter: 24px;
```
<hr>
### $mg-border-box
Matryosass uses `box-sizing: border-box` to keep everything in proportion. If you've already got this defined globally, or with normalize.css, then feel free to set this to false.
```scss
// @bool
$mg-border-box: true;
```
<hr>
## $mg-bp*
There are 5 breakpoints in Matryosass (4 max-width, 1 min-width). They work from smallest to largest.
#### $mg-bp-_n_
The first option is to enable/disable the breakpoint. If you don't need one, switch it to `false` to get a filesize reward.
```scss
// @bool
$mg-bp-xs          : true;
```
#### $mg-bp-_n_-width
Aptly named: define the width for the breakpoint.
```scss
// @int + unit
$mg-bp-xs-width    : 30em; /* ~480px */
```
#### $mg-bp-_n_-name
These will be the names you prefix to your column with modifiers, and also when you use the `@include media` mixin. By default these are named `xs`,`sm`,`md`,`lg`,`xl` for those familiar with Bootstrap - though I urge you to use naming conventions that work for __you__. 
```scss
// @string
$mg-bp-xs-name     : 'xs';
```

## Reversing, nesting, break-pointin'

To start with you'll need to wrap your grid in a class of `.grid`, this has a negative left margin (the same width as the column gutter) so you don't need to worry about any `.last` or `:nth-of-type:last` shenanigans. 

It is imperative you do not add any margins, widths or padding to the `.col` items themselves. They form the structure of your site, and you add content within them. To paraphrase Harry Roberts, "the grid and it's columns are the shelves on which you place your things."

A simple two-column layout with a 40%:60% split:
```html
<div class="grid">
  <div class="col-2-5">
    <!-- archive -->
  </div>
  <div class="col-3-5">
     <!-- blog post -->
  </div>
</div>
```
Now on a mobile device we probably want that display order to be reversed, so we'd use the `.grid--rev` class instead of `.grid`, which will float our `.col-` items to the right.

So what if we want a block of modules underneath the blog post, but within the same column? We nest. This is when fractional widths become so awesome, because we still have access to our whole range of widths - but within a new container! So let's have 3 evenly spaced out modules that are 1/3 of the width of the blog post column:
```html
...
  <div class="col-3-5">
     <!-- blog post -->
     
     <div class="grid">
      <!-- modules -->
      <div class="col-1-3">
        <!-- module 1 -->
      </div>
      <div class="col-1-3">
        <!-- module 1 -->
      </div>
      <div class="col-1-3">
        <!-- module 1 -->
      </div>
     </div>
  </div>
...
```
