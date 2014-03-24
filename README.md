# WTF, HTML and CSS?

Open an issue or a pull request to suggest changes or additions. Support for items listed below is not provided. Links to additional resources are provided whenever possible.

### Contents

1. [Set your doctype](#set-your-doctype)
2. [Box model math](#box-model-math)
3. [Floats come first](#floats-come-first)
4. [Floats and clearing](#floats-and-clearing)
5. [Floats and computed height](#floats-and-computed-height)
6. [Vertical margins often collapse](#vertical-margins-often-collapse)
7. [Styling table rows](#styling-table-rows)
8. [Firefox fubars `<input>` buttons](#firefox-fubars-input)
9. [Always set a `type` on `<button>`s](#always-set-a-type-on-buttons)
10. [Internet Explorer's selector limit](#internet-explorers-selectors)
11. [Position explained](#position-explained)
12. [Position and width](#position-and-width)
13. [`transform` and `position: fixed`](#transform-and-position-fixed)

-----

### Set your doctype
Always include the HTML5 doctype, `<!DOCTYPE html>`, otherwise you'll run into tons of issues like malformed tables, inputs, and the like.

### Box model math
Elements that have a set `width` become *wider* when they have `padding` and/or `border-width`. To avoid these problems, make use of the now common [`box-sizing: border-box` reset](http://www.paulirish.com/2012/box-sizing-border-box-ftw/).

### Floats come first
Floated elements should always come first in the document order. Floats have to have something to wrap around, otherwise they can cause a step down effect.

### Floats and clearing
If you float it, you *probably* need to clear it. Any content that follows an element with a `float` will wrap around that element until cleared. Use [the micro clearfix](http://nicolasgallagher.com/micro-clearfix-hack/) to clear your floats. Setting `overflow: [hidden|auto]` on the parent of a floated element with will also work, with some side affects.

### Floats and computed height
A parent element that has only floated content will have a computed `height: 0;`. Add a clearfix to the parent to trigger a proper computer height.

### Floated elements are block level
Any element with a `float` set automatically becomes `display: block`, thus you do not need to set a `float` and a `display`. Years ago we had to set `display: inline` for floats to work properly in IE6, but those days have passed.

### Vertical margins often collapse
Top and bottom margins can and will collapse in many situations, but never for floated or absolutely positioned elements. [Read this MDN article](https://developer.mozilla.org/en-US/docs/Web/CSS/margin_collapsing) to find out more. **Horizontal margins will never collapse.**

### Styling table rows
Table rows, `<tr>`s, cannot be styled unless you set `border-collapse: separate;` on the parent `<table>`.

### Firefox fubars `<input>` buttons
Don't use button or submit buttons (e.g., `<input type="button">...</input>` or `<input type="submit">...</input>`). Firefox still sets styles that cannot be overridden by custom CSS, thus causing these buttons to be taller than a `<button>` element.

### Always set a `type` on `<button>`s
The default value is `submit`, meaning any button in a form can submit the form. Use `type="button"` for anything that doesn't submit the form and `type="submit"` for those that do.

### Internet Explorer's selector limit
Internet Explorer 9 and below have a max of 4,096 selectors per stylesheet. Anything after this limit is ignored by the browser. Either split your CSS up, or start refactoring. I'd suggest the latter.

### Position explained
Elements with `position: fixed;` are placed relative to the browser viewport. Elements with `position: absolute;` are placed relative to their closest parent with a position other than `static` (e.g., `relative`, `absolute`, or `fixed`).

### Position and width
You don't need to set `width: 100%` on an element with `position: [absolute|fixed]` with both `left` and `right` values declared. Use either `width` or `left` and `right`, but not both.

### `transform` and `position: fixed`
`position: fixed` breaks when an element's parent has a `transform` set. Using transforms creates a new containing block, effectivly forcing the parent to have `position: relative` and the fixed element to behave as `position: absolute`. [See the demo](http://jsbin.com/yabek/1/) and read [Eric Meyer's post on the matter](http://meyerweb.com/eric/thoughts/2011/09/12/un-fixing-fixed-elements-with-css-transforms/).

-----

## Copyright and license
Copyright Mark Otto and released under the MIT license.
