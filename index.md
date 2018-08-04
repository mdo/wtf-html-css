---
layout: default
---

### Contents

- [Декларуйте doctype](#doctype)
- [Математика box моделі](#box-model-math)
- [Rem одиниці та мобільний Safari](#rems-mobile-safari)
- [Спочатку float-и](#floats-first)
- [Float-и та очищення](#floats-clearing)
- [Float-и і обчислювана висота](#floats-computed-height)
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
### Декларуйте doctype
Завжди додавайте doctype. Я рекомендую простий HTML5 doctype:

```html
<!DOCTYPE html>
```

[Упускання doctype може спричиняти помилки](http://quirks.spec.whatwg.org) з неправильними таблицями, формами тощо, оскільки сторінка буде виведена в примхливому режимі.


<a name="box-model-math"></a>
### Математика box моделі
Елементи, що мають встановлену ширину `width` стають *ширшими* коли вони мають встановлені `padding` та/чи `border-width`. Аби уникнути цих проблем, використовуйте тепер повсякденне [`box-sizing: border-box;` скидання](http://www.paulirish.com/2012/box-sizing-border-box-ftw/).


<a name="rems-mobile-safari"></a>
### Rem одиниці та мобільний Safari
Хоча мобільний Safari підтримує використання `rem`-ів у всіх властивостях, він, схоже, нервово курить в сторонці, коли використовуються `rem` в просторових мультимедійних запитах і нескінченно миготить текст сторінки в різних розмірах.

Поки що, використовуйте `em`-и замість `rem`-ів.

```css
html {
  font-size: 16px;
}

/* Спричиняє миготіння в мобільному Safari */
@media (min-width: 40rem) {
  html {
    font-size: 20px;
  }
}

/* Прекрасно працює в мобільному Safari */
@media (min-width: 40em) {
  html {
    font-size: 20px;
  }
}
```

**Допоможіть!** *Якщо у вас є посилання на повідомлення про помилку Apple або WebKit, я хотів би його включити. Я не впевнений, куди повідомити про це, оскільки баг стосується лише мобільного, а не десктопного Safari.*


<a name="floats-first"></a>
### Спочатку float-и
Плаваючі елементи повинні завжди бути першими в порядку документа. Плаваючі елементи зобов'язані щось обернути, інакше вони можуть спричинити ефект випадання, а не з'являтиметься нижче вмісту.

```html
<div class="parent">
  <div class="float">Плаваємо</div>
  <div class="content">
    <!-- ... -->
  </div>
</div>
```


<a name="floats-clearing"></a>
### Float-и та очищення
Якщо ви пускаєте щось у плавання, то, *можливо*, варто щось очистити. Будь-який вміст, який йде за елементом з "float", обертається навколо цього елемента до моменту очищення. Щоб очистити float-и, використовуйте однин з наступних методів.

Використовуйте [micro clearfix](http://nicolasgallagher.com/micro-clearfix-hack/), аби очистити ваші float-и за допомогою окремого класу.

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

З іншого боку, можна вказати `overflow`, з параметрами `auto` чи `hidden`, для батьківського елемента.

```css
.parent {
  overflow: auto; /* clearfix */
}
.other-parent {
  overflow: hidden; /* clearfix */
}
```

Майте на увазі, що `overflow` може спричинити інші небажані побічні ефекти, як правило, навколо розташованих елементів у батьківському елементі.

**Професійна порада!** *Зберігайте свої нерви на майбутнє та ваших колег задоволенням, включивши коментар на кшталт `/* clearfix */` під час очищення float-ів, оскільки властивість може бути використана з інших причин.*


<a name="floats-computed-height"></a>
### Float-и і обчислювана висота
Батьківський елемент, який складається лише з float-ів, матиме висоту `height: 0;`. Додайте clearfix для батьківського елементу, аби змусити браузерів обчислювати висоту.


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

**Good news!** *It looks like [a fix for this](https://bugzilla.mozilla.org/show_bug.cgi?id=697451#c43) might be coming in Firefox 30. That's good news for our future selves, but be aware this doesn't fix older versions.*


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
