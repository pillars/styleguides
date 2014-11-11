# CSS Style Guide

All code in any code-base should look like a single person typed it, no matter how many people contributed.

> "Arguments over style are pointless. There should be a style guide, and you  should follow it"
> Rebecca Murphey

This guide is inspired by those guides:

- https://github.com/styleguide/css
- https://github.com/ginatrapani/ThinkUp/wiki/Code-Style-Guide:-CSS
- https://github.com/necolas/idiomatic-css
- http://google-styleguide.googlecode.com/svn/trunk/htmlcssguide.xml
- https://gist.github.com/fat/b27700946c777adacdf4

## Table of Contents

- [Convention](#conventions)
- [Strings](#strings)
- [Comments](#comments)

---

## Conventions

### No `#id` selectors

Do not use `#id` selectors ever. If you want to know why, [read this](http://oli.jp/2011/ids/).

### No vendor prefixes

We have [autoprefixer](https://github.com/postcss/autoprefixer) to take care of that. Do not pollute the code with ugly `-moz`, `-webkit`, ...

### No HTML inline style

Only use HTML inline style (`style=""`) if you don't have a choice. It pretty much limits its use to database generated CSS.

### Single quotes

Always use single quotes `'`:

```css
/* bad - no quotes */
.foo[type=checkbox] {}

/* bad - double quotes */
.foo[type="checkbox"] {
  font-family: "open sans", arial, sans-serif;
}

/* good */
.foo[type='checkbox'] {
  font-family: 'open sans', arial, sans-serif;
}
```

Exception: If you do need to use the `@charset` rule, use double quotation marks — single quotation marks are not permitted.

### Semicolon `;`

Always add a semicolon at the end of a property declaration, even if it is the last one in a rule declaration:

```css
/* bad - no semicolon */
.foo {
  width: 72px;
  height: 72px
}

/* good */
.foo {
  width: 72px;
  height: 72px;
}
```

### Type Selectors

Unless necessary (for example with helper classes), do not use element names in conjunction with IDs or classes:

```css
/* bad */
ul.foo {}

/* good */
.foo {}
```

### Shorthand Properties

CSS offers a variety of [shorthand](http://www.w3.org/TR/CSS21/about.html#shorthand) properties (like font) that should be used whenever possible, even in cases where only one value is explicitly set:

```
/* bad */
border-top-style: none;
font-family: palatino, georgia, serif;
font-size: 100%;
line-height: 1.6;
padding-bottom: 2em;
padding-left: 1em;
padding-right: 1em;
padding-top: 0;

/* good */
border-top: 0;
font: 100%/1.6 palatino, georgia, serif;
padding: 0 1em 2em;
```

### Whitespaces

#### Base rules:

- Use 2 spaces (soft tabs). Never use tabs.
- Line length must never exceed **80 characters**.
- File must end with a **single** new line.
- Trailing whitespaces must be trimmed.

#### Spaces

- Avoid single line rule declarations.
- Add a semicolon after every property declarations.
- One property declaration per line.
- Put spaces after `:` in property declarations.
- Put spaces before `{` in rule declarations.
- Put line breaks between rulesets.
- When grouping selectors, keep individual selectors to a single line.
- Place closing braces of declaration blocks on a new line.
- Each declaration should appear on its own line for more accurate error reporting.

**Bad**

```css
/* bad - no spaces */
.foo{
  height:72px;
}

/* bad - no semicolon */
.foo {
  height: 72px
}

/* bad - two rules on a single line */
.foo {
  height: 72px; width: 100px;
}

/* bad - {} improperly placed */
.foo {
  height: 72px; }

.foo
{
  height: 72px;
}

.foo {
  height: 72px;
     }

/* bad - two selectors on a single line */
.foo, .bar {
  height: 72px;
}

/* bad - no space between rule declarations */
.foo {
  height: 72px;
}
.bar {
  height: 72px;
}

/* bad - two spaces between rule declarations */
.foo {
  height: 72px;
}


.bar {
  height: 72px;
}

/* bad - inline declarations */
.foo { height: 72px; }
```

**Good**

```css
.foo,
.bar {
  height: 72px;
  width: 100px;
}

.bar {
  color: blue;
}
```

### Multi values properties

For properties with multiple values, separate each value with a single space following the comma(s).

```
font-family: Helvetica, sans-serif;
```

If a single value contains any spaces, that value must be enclosed within double quotation marks:

```
font-family: "Lucida Grande", Helvetica, sans-serif;
```

Long, comma-separated property values - such as collections of gradients or shadows - can be arranged across multiple lines in an effort to improve readability and produce more useful diffs. There are various formats that could be used; one example is shown below.

```css
.foo {
    background-image:
        linear-gradient(#fff, #ccc),
        linear-gradient(#f3c, #4ec);
    box-shadow:
        1px 1px 1px #000,
        2px 2px 1px 1px #ccc inset;
}
```

### Rule order

Smaller teams may prefer to cluster related properties (e.g. positioning and box-model) together.

```css
.selector {
    /* Positioning */
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    z-index: 10;

    /* Display & Box Model */
    border: 10px solid #333;
    box-sizing: border-box;
    display: inline-block;
    margin: 10px;
    padding: 10px;
    overflow: hidden;
    width: 100px;
    height: 100px;

    /* Other */
    background: #000;
    color: #fff;
    font-family: sans-serif;
    font-size: 16px;
    text-align: right;
}
```

Larger teams may prefer the simplicity and ease-of-maintenance that comes with alphabetical ordering.

### Nesting

Indent nested properties and use line spaces:

```css
/* bad */
@media screen, projection {
html {
  background: #fff;
  color: #444;
}
}

/* good */
@media screen, projection {

  html {
    background: #fff;
    color: #444;
  }

}
```

Limit nesting to 1 level deep. Reassess any nesting more than 2 levels deep. This prevents overly-specific CSS selectors.

Avoid large numbers of nested rules. Break them up when readability starts to be affected. Preference to avoid nesting that spreads over more than 20 lines.

OR ????

Don't nest. Ever.

Nesting makes it harder to tell at a glance where css selector optimizations can be made.

```css
/* bad */
.list-btn {
  .list-btn-inner {
    .btn {
      background: red;
    }
    .btn:hover {
      .opacity(.4);
    }
  }
}

/* good */
.list-btn .btn-inner {
  background: red;
}
.list-btn .btn-inner:hover {
  .opacity(.4);
}
```

??? Could we nest states ? Like `hover` 

---

## Comments

Use `/* */` for comment blocks (instead of `//`).

### Code blocks

```css
/* ==========================================================================
   Section comment block
   ========================================================================== */

/* Sub-section comment block
   ========================================================================== */

// Sub-section comment block // ---------------------------------------------

/* Title of section
 **********************/
```

### HTML description

Consider the following html:

```
<div class="carousel">
    <ul class="panes">
        <li class="pane">
            <h2 class="pane-title">Bla</h2>
        </li>
        <li class="pane">
            <h2 class="pane-title">Bla</h2>
        </li>
    </ul>
</div>
```

When you style the `.carousel`, comment the html structure:

```
/**
 * .carousel
 *
 * The first sentence of the long description starts here and continues on this
 * line for a while finally concluding here at the end of this paragraph.
 *
 * div.carousel
 *   ul.panes
 *     li.pane
 *       h2.pane-title
 */
```

---

## Naming

Class names and attributes must be lowercase:

```css
/* bad */
.P {}
.Foo{}

/* good */
p {}
.foo {}
```

Use dashes to separate multi word selector:

```
/* bad */
.foobar
.fooBar
.FooBar
.foo_bar

/* good */
.foo-bar
```

### Component

In large projects as well as for code that gets embedded in other projects or on external sites use prefixes (as namespaces) for class names. Use short, unique identifiers followed by a dash.

Using namespaces helps preventing naming conflicts and can make maintenance easier, for example in search and replace operations.

### State classes

If you want to define a class that describes a state, prefix it with `is-`.

### Javascript classes

Using css classes for both Javascript action and styling is highly error prone. There should be no coupling between actions and styling. If you define a class to make the element actionable via Javascript, prefix the class with `js-`. 

```
.js-nav-open
```

That class should not be used for styling at all. If you need to style the element, add another classname without the prefix and style the one without the prefix:

```
.js-nav-open.nav-open
```

Hint: We don't use `data-` attributes to handle actions instead of mixing styling classes and action classes because of [performance](http://jsperf.com/data-selector-performance-without-values).

---

## Units

When denoting the dimensions — that is, the width or height — of an element or its margins, borders, or padding, specify the units in either `em`, `px`, or `%`. If the value is 0, do not specify units.

```
/* bad - no unit */
width: 12;

/* bad - unit on 0 value */
width: 0px;

/* good */
width: 12px;
width: 12%;
width: 12em;
width: 12rem;
width: 0;
```


### Leading 0s

Do not use put 0s in front of values or lengths between -1 and 1.

```
/* bad */
font-size: 0.8em;

/* good */
font-size: .8em;
```

### Pixels vs ems vs rems

Use px for font-size, because it offers absolute control over text. Additionally, unit-less line-height is preferred because it does not inherit a percentage value of its parent element, but instead is based on a multiplier of the font-size.

---

## Images

Name images the same way you name selectors:

```
/* bad */
iconhome.png
iconHome.png
icon_home.png

/* good */
icon-home.png
```

Images must be prefixed with their usage:

```
icon-home.png
bg-home.jpg
sprite-top-navigation.png
```

We sprite

Fonts

---

## Colors

Use hex color codes `#000` unless using `rgba()`.

```
/* bad - don't use rgb */
color: rgb(255, 255, 255);
color: white;
color: hsl(120, 100%, 50%);
color: hsla(120, 100%, 50%, 1);

/* good */
color: #FFF;
color: rgba(255, 255, 255, .8);
```

When denoting color using hexadecimal notation, use all capital letters. Both three-digit and six-digit hexadecimal notation are acceptable; if it’s possible to specify the desired color using three-digit hexadecimal notation, do so as you’ll save the end-user a few bytes of download time.

```
/* bad - lowercase */
color: #fff;

/* bad - use short version */
color: #FFFFFF;

/* good */
color: #FE9848;
color: #FFF;
```

Also avoid to define colors directly in the code. Instead use color variable defined in `colors.scss`.

```

```

---

## Fonts

With the additional support of web fonts `font-weight` plays a more important role than it once did. Different font weights will render typefaces specifically created for that weight, unlike the old days where `bold` could be just an algorithm to fatten a typeface. Obvious uses the numerical value of `font-weight` to enable the best representation of a typeface. The following table is a guide:

Raw font weights should not be specified. Instead, use the appropriate font mixin: `.wf-sans-i7, .wf-sans-n7, etc.`

The suffix defines the weight and style:

```CSS
n = normal
i = italic
4 = normal font-weight
7 = bold font-weight
```

Refer to type.less for type size, letter-spacing, and line height. Raw sizes, spaces, and line heights should be avoided outside of type.less.


```
$type-micro
$type-smallest
$type-smaller
$type-small
$type-base
$type-large
$type-larger
$type-largest
$type-jumbo
```

---

## z-index

We don't want a `z-index` of 10000000. Actually if you find yourself entering a numeric value for `z-index`, you are doing something wrong. We have a scale of 10 `z-index` variables ranging from 100 to 1000. This scale is not to be touched.

Now if you want to create a component and need to position it, you create a `$zIndex-1--carousel` variable in the `z-index.scss` file and position it where it belongs. This file give you a quick glance of the application structure and how things are organized relatively.

```
// z-index Scale (do not edit)
// --------------------------------------------------
$zIndex-1:   100;
$zIndex-2:   200;
$zIndex-3:   300;
$zIndex-4:   400;
$zIndex-5:   500;
$zIndex-6:   600;
$zIndex-7:   700;
$zIndex-8:   800;
$zIndex-9:   900;
$zIndex-10: 1000;


// z-index Applications
// --------------------------------------------------
$zIndex-1--something:               @zIndex-1;

$zIndex-2--component:               @zIndex-2;
```

---

## URLs

Do not set the protocol on URLs:

```css
/* bad */
background: url(http://www.exemple.com);

/* good */
background: url(//www.google.com);
```

Use quotation marks in URI values (url()):

```
/* bad */
@import url(//www.exemple.com);
@import url("//www.exemple.com");

/* good */
@import url('//www.exemple.com');
```

--- 

## File structure

```
styles
├── base
│   ├── text.css
│   └── table.css
├── config
│   ├── colors.css
│   └── z-index.css
├── components
│   ├── carousel.css
│   └── tooltip.css
├── layouts
│   ├── page.css
│   ├── page-body.css
│   ├── page-foot.css
│   ├── page-head.css
│   └── page-sidebar.css
├── shared
│   └── helper.css
├── vendors
│   └── select2.css
└── views
    ├── home.css
    └── contact.css
```

### Componentizing

Always look to abstract components. Medium has a very strong, very consistent style and the reuse of components across designs helps to improve this consistency at an implementation level.

A name like `.homepage-nav` limits its use. Instead think about writing styles in such a way that they can be reused in other parts of the app. Instead of `.homepage-nav`, try instead `.nav` or `.nav-bar`. Ask yourself if this component could be reused in another context (chances are it could!).

Components should belong to their own less file. For example, all general button definitions should belong in buttons.less.

## Name-spacing

Name-spacing is great! But it should be done at a component level â€“ never at a page level.

Also, namespacing should be made at a descriptive, functional level. Not at a page location level. For example, `.profile-header` could become `.header-hero-unit`.

**Wrong:**

```css
.nav,
.home-nav,
.profile-nav,
```

**Right:**

```css
.nav,
.nav-bar,
.nav-list
```

## Style Scoping

Medium pages should largely be reusing the general component level styles defined above. Page level name-spaces however can be helpful for overriding generic components in very specific contexts.

Page level overrides should be minimal and under a single page level class nest.

```CSS
.home-page {
  .nav {
    margin-top: 10px;
  }
}
```

### Mixins

Make sure to take into consideration the output of using LESS' powerful mixins. They are best used for grouping browser-specific code or as powerful ways to contain functionality. They are not a good way to add additional styles to an element as you will be sending duplicate styles across the wire.

**Wrong:**

```html
<button class="btn-secondary">Click me</button>
```

```css
.btn-primary {
  color: red;
  background: black;
  width: 120px;
  height: 60px
  border-radius: 6px
}
.btn-secondary {
  .btn-primary;
  color: green;
}
.btn-disabled {
  .btn-primary;
  color: gray;
  background: darkgray;
}
```

Generated Output:
```css
.btn-primary {
  color: red;
  background: black;
  width: 120px;
  height: 60px
  border-radius: 6px
}
.btn-secondary {
  color: red;
  background: black;
  width: 120px;
  height: 60px
  border-radius: 6px
  color: green;
}
.btn-disabled {
  color: red;
  background: black;
  width: 120px;
  height: 60px
  border-radius: 6px
  color: gray;
  background: darkgray;
}
```

**Right:**

```html
<button class="btn btn-secondary">Click me</button>
```

```css
.btn {
  width: 120px;
  height: 60px
  border-radius: 6px
}
.btn-primary {
  color: red;
  background: black;
}
.btn-disabled {
  color: gray;
  background: darkgray;
}
```

Generated Output:
```css
.btn {
  width: 120px;
  height: 60px
  border-radius: 6px
}
.btn-primary {
  color: red;
  background: black;
}
.btn-disabled {
  color: gray;
  background: darkgray;
}
```
