---
layout: default
---

### Contents

- [Declare a doctype](#doctype)
- [Box model math](#box-model-math)
- [Rem units and Mobile Safari](#rems-mobile-safari)
- [Floats first](#floats-first)
- [Floats and clearing](#floats-clearing)
- [Floats and computed height](#floats-computed-height)
- [Floated are block level](#floats-block-level)
- [Vertical margins often collapse](#vertical-margins-collapse)
- [Styling table rows](#styling-table-rows)
- [Firefox and `<input>` buttons](#buttons-firefox)
- [Firefox inner outline on buttons](#buttons-firefox-outline)
- [Always set a `type` on `<button>`s](#buttons-type)
- [Internet Explorer's selector limit](#ie-selector-limit)
- [Position explained](#position-explained)
- [Position and width](#position-width)
- [Fixed position and transforms](#position-transforms)


<a name="doctype"></a>
### Declare a doctype
Always include a doctype. I recommend the simple HTML5 doctype:

```html
<!DOCTYPE html>
```

[Skipping the doctype can cause issues](http://quirks.spec.whatwg.org) with malformed tables, inputs, and more as the page will be rendered in quirks mode.


<a name="box-model-math"></a>
### Box model math
Elements that have a set `width` become *wider* when they have `padding` and/or `border-width`. To avoid these problems, make use of the now common [`box-sizing: border-box;` reset](http://www.paulirish.com/2012/box-sizing-border-box-ftw/).


<a name="rems-mobile-safari"></a>
### Rem units and Mobile Safari
While Mobile Safari supports the use of `rem`s in all property values, it seems to shit the bed when `rem`s are used in dimensional media queries and infinitely flashes the page's text in different sizes.

For now, use `em`s in place of `rem`s.

```css
html {
  font-size: 16px;
}

/* Causes flashing bug in Mobile Safari */
@media (min-width: 40rem) {
  html {
    font-size: 20px;
  }
}

/* Works great in Mobile Safari */
@media (min-width: 40em) {
  html {
    font-size: 20px;
  }
}
```

**Help!** *If you have a link to an Apple or WebKit bug report for this, I'd love to include it. I'm unsure where to report this as it only applies to Mobile, and not Desktop, Safari.*


<a name="floats-first"></a>
### Floats first
Floated elements should always come first in the document order. Floated elements require something to wrap around, otherwise they can cause a step down effect, instead appearing below the content.

```html
<div class="parent">
  <div class="float">Float</div>
  <div class="content">
    <!-- ... -->
  </div>
</div>
```


<a name="floats-clearing"></a>
### Floats and clearing
If you float it, you *probably* need to clear it. Any content that follows an element with a `float` will wrap around that element until cleared. To clear floats, use one of the following techniques.

Use [the micro clearfix](http://nicolasgallagher.com/micro-clearfix-hack/) to clear your floats with a separate class.

```css
.clearfix:before,
.clearfix:after {
  display: table;
  content: "";
}
.clearfix:after {
  clear: both;
}
```

Alternatively, specify `overflow`, with `auto` or `hidden`, on the parent.

```css
.parent {
  overflow: auto; /* clearfix */
}
.other-parent {
  overflow: hidden; /* clearfix */
}
```

Be aware that `overflow` can cause other unintended side effects, typically around positioned elements within the parent.

**Pro-Tip!** *Keep your future self and your coworkers happy by including a comment like `/* clearfix */` when clearing floats as the property can be used for other reasons.*


<a name="floats-computed-height"></a>
### Floats and computed height
A parent element that has only floated content will have a computed `height: 0;`. Add a clearfix to the parent to force browsers to compute a height.


<a name="floats-block-level"></a>
### Floated elements are block level
Elements with a `float` will automatically become `display: block;`. Do not set both as there is no need and the `float` will override your `display`.

```css
.element {
  float: left;
  display: block; /* Not necessary */
}
```

**Fun fact:** *Years ago, we had to set `display: inline;` for most floats to work properly in IE6 to avoid the [double margin bug](http://www.positioniseverything.net/explorer/doubled-margin.html). However, those days have long passed.*


<a name="vertical-margins-collapse"></a>
### Vertically adjacent margins collapse
Top and bottom margins on adjacent elements (one after the other) can and will collapse in many situations, but never for floated or absolutely positioned elements. [Read this MDN article](https://developer.mozilla.org/en-US/docs/Web/CSS/margin_collapsing) or the CSS2 spec's [collapsing margin section](http://www.w3.org/TR/CSS2/box.html#collapsing-margins) to find out more.

Horizontally adjacent margins will **never collapse**.


<a name="styling-table-rows"></a>
### Styling table rows
Table rows, `<tr>`s, do not receive `border`s unless you set `border-collapse: collapse;` on the parent `<table>`. Moreover, if the `<tr>` and children `<td>`s or `<th>`s have the *same* `border-width`, the rows will not see their border applied. [See this JS Bin link for an example.](http://jsbin.com/yabek/2/)


<a name="buttons-firefox"></a>
### Firefox and `<input>` buttons

For reasons unknown, Firefox applies a `line-height` to submit and button `<input>`s that cannot be overridden with custom CSS. You have two options in dealing with this:

1. Stick to `<button>` elements
2. Don't use `line-height` on your buttons

Should you go with the first route (and I recommend this one anyway because `<button>`s are great), here's what you need to know:

```html
<!-- Not so good -->
<input type="submit" value="Save changes">
<input type="button" value="Cancel">

<!-- Super good everywhere -->
<button type="submit">Save changes</button>
<button type="button">Cancel</button>
```

Should you wish to go the second route, just don't set a `line-height` and use *only* `padding` to vertically align button text. [View this JS Bin example](http://jsbin.com/yabek/4/) in Firefox to see the original problem and the workaround.

**Good news!** *Mozilla [fixed this](https://bugzilla.mozilla.org/show_bug.cgi?id=697451#c43) since Firefox 30 (June 2014). That's good news, but be aware this doesn't fix older versions.*


<a name="buttons-firefox-outline"></a>
### Firefox inner outline on buttons

Firefox [adds an inner outline](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/button#Notes) to buttons (`<input>`s and `<button>`s) on `:focus`. Apparently it's for accessibility, but its placement seems rather odd. Use this CSS to override it:

```css
input::-moz-focus-inner,
button::-moz-focus-inner {
  padding: 0;
  border: 0;
}
```

You can see this fix in action in the same [JS Bin example](http://jsbin.com/yabek/4/) mentioned in the previous section.

**Pro-Tip!** *Be sure to include some focus state on buttons, links, and inputs. Providing an affordance for accessibility is paramount, both for pro users who tab through content and those with vision impairments.*


<a name="buttons-type"></a>
### Always set a `type` on `<button>`s
The default value is `submit`, meaning any button in a form can submit the form. Use `type="button"` for anything that doesn't submit the form and `type="submit"` for those that do.

```html
<button type="submit">Save changes</button>
<button type="button">Cancel</button>
```

For actions that require a `<button>` and are not in a form, use the `type="button"`.

```html
<button class="dismiss" type="button">x</button>
```

**Fun fact:** *Apparently IE7 doesn't properly support the `value` attribute on `<button>`s. Instead of reading the attribute's content, it pulls from the innerHTML (the content between the opening and closing `<button>` tags). However, I don't see this as a huge concern for two reasons: IE7 usage is way down, and it seems rather uncommon to set both a `value` and the innerHTML on `<button>`s.*


<a name="ie-selector-limit"></a>
### Internet Explorer's selector limit
Internet Explorer 9 and below have a max of 4,096 selectors per stylesheet. There is also a limit of 31 combined stylesheets and `<style></style>` includes per page. Anything after this limit is ignored by the browser. Either split your CSS up, or start refactoring. I'd suggest the latter.

As a helpful side note, here's how browsers count selectors:

```css
/* One selector */
.element { }

/* Two more selectors */
.element,
.other-element { }

/* Three more selectors */
input[type="text"],
.form-control,
.form-group > input { }
```


<a name="position-explained"></a>
### Position explained
Elements with `position: fixed;` are placed relative to the browser viewport. Elements with `position: absolute;` are placed relative to their closest parent with a position other than `static` (e.g., `relative`, `absolute`, or `fixed`).


<a name="position-width"></a>
### Position and width
Don't set `width: 100%;` on an element that has `position: [absolute|fixed];`, `left`, and `right`. The use of `width: 100%;` is the same as the combined use of `left: 0;` and `right: 0;`. Use one or the other, but not both.


<a name="position-transforms"></a>
### Fixed position and transforms
Browsers break `position: fixed;` when an element's parent has a `transform` set. Using transforms creates a new containing block, effectively forcing the parent to have `position: relative;` and the fixed element to behave as `position: absolute;`.

[See the demo](http://jsbin.com/yabek/1/) and read [Eric Meyer's post on the matter](http://meyerweb.com/eric/thoughts/2011/09/12/un-fixing-fixed-elements-with-css-transforms/).
