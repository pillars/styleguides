NEVER OVERWRITE VENDOR FILES!!!
bitter in base



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

- [Conventions](#conventions)
- [Comments](#comments)
- [Units](#units)
- [Colors](#colors)
- [Fonts](#fonts)
- [ZIndex](#zindex)
- [File structure](#file-structure)

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

### This is not `!important`

Don't rely on using `!important` to override styles. Either make the styles to be overridden more generic or make the overriding styles more specific.

### Type Selectors

Unless necessary (for example with helper classes), do not use element names in
conjunction with IDs (which you're not supposed to use anyway) or classes:

```css
/* bad */
ul.foo {}

/* good */
.foo {}
```

### Shorthand Properties

CSS offers a variety of [shorthand](http://www.w3.org/TR/CSS21/about.html#shorthand) properties (like font) that should be used whenever possible, even in cases where only one value is explicitly set:

```css
/* bad */
selector {
  border-top-style: none;
  font-family: palatino, georgia, serif;
  font-size: 100%;
  line-height: 1.6;
  padding-bottom: 2em;
  padding-left: 1em;
  padding-right: 1em;
  padding-top: 0;
}

/* good */
selector {
  border-top: 0;
  font: 100%/1.6 palatino, georgia, serif;
  padding: 0 1em 2em;
}
```

### Whitespaces

- Use 2 spaces (soft tabs). Never use tabs.
- Line length must never exceed **80 characters**.
- File must end with a **single** new line.
- Trailing whitespaces must be trimmed.

And for the syntax of rule declarations:

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

### Naming

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

```css
/* bad */
.foobar {}
.fooBar {}
.FooBar {}
.foo_bar {}

/* good */
.foo-bar {}
```

#### Components

In large projects as well as for code that gets embedded in other projects or on
external sites use prefixes (as namespaces) for class names. Use short, unique
identifiers followed by a dash.

Using namespaces helps preventing naming conflicts and can make maintenance
easier, for example in search and replace operations.

#### State classes

If you want to define a class that describes a state, prefix it with `is-`.

#### Javascript classes

Using css classes for both Javascript action and styling is highly error prone.
There should be no coupling between actions and styling. If you define a class
to make the element actionable via Javascript, prefix the class with `js-`.

```css
.js-nav-open {}
```

That class should not be used for styling at all. If you need to style the
element, add another classname without the prefix and style the one without the
prefix:

```css 
.js-nav-open.nav-open {}
```

Hint: We don't use `data-` attributes to handle actions instead of mixing
styling classes and action classes because of
[performance](http://jsperf.com/data-selector-performance-without-values).

#### Images

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

### Multi values properties

For properties with multiple values, separate each value with a single space following the comma(s).

```
font-family: Helvetica, sans-serif;
```

If a single value contains any spaces, that value must be enclosed within single quotation marks:

```
font-family: 'Lucida Grande', Helvetica, sans-serif;
```

Long, comma-separated property values - such as collections of gradients or
shadows - can be arranged across multiple lines in an effort to improve
readability and produce more useful diffs:

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

Group related properties (e.g. positioning and box-model) together like shown below. Include mixins at the top of the section it belongs to. 

```css
.selector {
    /* Positioning */
    float:;
    clear:;
    position:;
    top:;
    right:;
    bottom:;
    left:;
    z-index:;

    /* Display & Box Model */
    border:;
    box-sizing:;
    display:;
    margin:;
    padding:;
    overflow:;
    width:;
    height:;

    /* Visual Styling */
    background:;
    border-radius:;
    box-shadow:;
    content:;

    /* Text / Font */
    color:;
    font:;
    line-height:;
    text-align:;

    /* Transitions */
    transitions:;
}
```

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

Try to limit nesting to 1 level deep. Reassess any nesting more than 2 levels
deep. This prevents overly-specific CSS selectors.

Avoid large numbers of nested rules. Break them up when readability starts to
be affected. Preference to avoid nesting that spreads over more than 20 lines.

### URLs

Do not set the protocol on URLs:

```css
/* bad */
background: url(http://www.exemple.com);

/* good */
background: url(//www.google.com);
```

Do not use quotation marks in URI values (url()):

```
/* bad */
@import url('//www.exemple.com');
@import url("//www.exemple.com");

/* good */
@import url(//www.exemple.com);
```

---

## Comments

Use `/* */` for comment blocks (instead of `//`). Always use valid Markdown.

### Code blocks

**Bad**

```css
/* Sub-section comment block
   ========================================================================== */

// Sub-section comment block // ---------------------------------------------

/* Title of section
 **********************/
```

**Good**

```css
/** Section comment block
 *  ===================== */

/** Section comment block
 *  =====================
 *
 *  This is some stuff for some stuff.
 */
```

### HTML description

Consider the following html:

```html
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

When you style the `.carousel`, as it is a resuable component, document the html
structure:

```css
/** .carousel
 *  =========
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

## Units

When denoting the dimensions — that is, the width or height — of an element or
its margins, borders, or padding, specify the units in either `em`, `px`,
or `%`. If the value is 0, do not specify units.

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

Always put 0s in front of values or lengths between -1 and 1.

```
/* bad */
font-size: .8em;

/* good */
font-size: 0.8em;
```

### Pixels vs ems vs rems

Use px for font-size, because it offers absolute control over text.
Additionally, unitless line-height is preferred because it does not inherit
a percentage value of its parent element, but instead is based on a multiplier
of the font-size.

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
color: rgba(255, 255, 255, 0.8);
```

When denoting color using hexadecimal notation, use all lowercase letters. Both
three-digit and six-digit hexadecimal notation are acceptable; if it’s possible
to specify the desired color using three-digit hexadecimal notation, do so.

```
/* bad - uppercase */
color: #FFF;

/* bad - use short version */
color: #ffffff;

/* good */
color: #fe9848;
color: #fff;
```

Also avoid defining colors directly in the code. Instead use color variable
defined in `colors.scss`.

---

## Fonts

Refer to type.less for type size, letter-spacing, and line height. Raw sizes,
spaces, and line heights should be avoided outside of `_type.scss`.

Use the following naming for font-size abstraction:

```
type-xx-small
type-x-small
type-small
type-normal
type-large
type-x-large
type-xx-large
```

---

## ZIndex

We don't want a `z-index` of 10000000. Actually if you find yourself entering a
numeric value for `z-index`, you are doing something wrong. We have a scale of
10 `z-index` variables ranging from 100 to 1000. This scale is not to be
touched.

Now if you want to create a component and need to position it, you create a
`$zIndex-1--carousle` variable in the `z-index.scss` file and position it where
it belongs. This file give you a quick glance of the application structure and
how things are organized relatively.

```
/** z-index Scale (do not edit)
 *  =========================== */
$z-index-1  : 100;
$z-index-2  : 200;
$z-index-3  : 300;
$z-index-4  : 400;
$z-index-5  : 500;
$z-index-6  : 600;
$z-index-7  : 700;
$z-index-8  : 800;
$z-index-9  : 900;
$z-index-10 : 1000;


/** z-index Applications
 *  ==================== */
$z-index-1-something     : $z-index-1;

$z-index-2-something     : $z-index-2;
$z-index-2-somethingElse : $z-index-2;
```

---

## File structure

The project uses the following technologies: SASS, Bourbon and Bitters, Normalize.

```
styles
├── base
│   ├── forms.scss
│   ├── lists.scss
│   ├── table.scss
│   ├── text.scss
│   ├── colors.scss
│   ├── fonts.scss
│   └── z-index.scss
├── components
│   ├── carousel.scss
│   └── tooltip.scss
├── layouts
│   ├── page.scss
│   ├── page-body.scss
│   ├── page-foot.scss
│   ├── page-head.scss
│   └── page-sidebar.scss
├── shared
│   ├── buttons.scss
│   ├── clearfix.scss
│   └── hide-text.scss
├── variables
│   ├── colors.scss
│   ├── type.scss
│   └── zindex.scss
├── vendors
│   ├── bourbon/
│   └── normalize.scss
└── views
    ├── home.scss
    └── contact.scss
```

### Variables

The `variables/` folder welcomes configuration for your project:

- `colors.scss`: the color scale
- `fonts.scss`: the font families used in the app
- `zindex.scss`: the zindex scale

### Shared

The `shared/` folder welcomes mixins and placeholders for your project. Theses could be used anywhere in the application styles.

### Base

The goal is to define a consistent foundation across browsers to build the site on. So we normalize those elements in the `base/` folder. We use [normalize](http://necolas.github.io/normalize.css/) as a starting point and build on top of it to define the default look we want to give to elements:

- `forms.scss`: anything related to forms
- `lists.scss`: anything related to lists (ol, ul , dl)
- `tables.scss`: anything related to tables
- `text.scss`: anything related to headings and paragraphs

The rule is that you can't define rule declarations with a class or id. You must use html tags only:

```css
/* bad */
.paragraph {}

/* good */
p {}
```

The only exception is if you want to create a class named after an html attribute that reproduces it's behaviour:

```css
strong,
.strong {
  font-weight: bold;
}
```

### Layouts

If you consider a page, there always big structural parts like a `header`, a `foot`, a `body`, a `sidebar`... The layout folder host style for those high level elements. As a convention, we prefix those elements with the `page-` prefix:

- `page.scss`: style the viewport `body`, `html`
- `page-head.scss`: style the header `.page-head`
- `page-body.scss`: style the body `.page-body`
- `page-foot.scss`: style the footer `.page-foot`
- `page-sidebar.scss`: style the sidebar `.page-sidebar`
- ...

The list will depend on your page layout.

The `layout/` folder let you define multiple layouts for those elements. For instance, you may want to have both a fixed and fluid layout. You would then create two classes that could be applied to the body html tag `body.layout-fluid` and `body.layout-fixed`.

```css
.page-body {
  width: 960px;
  margin: 0 auto;
}

.layout-fluid .page-body {
  width: 100%;
  margin: 0;
}
```

### Components

Always look to abstract components. A name like `.homepage-nav` limits its use. Instead think about writing styles in such a way that they can be reused in other parts of the app. Instead of `.homepage-nav`, try instead `.nav` or `.nav-bar`. Ask yourself if this component could be reused in another context (chances are it could!). Every component should be stored in its own `file.scss`.

#### Naming

Name-spacing is great! But it should be done at a component level, never at a page level. It should be made at a descriptive, functional level. Not at a
page location level. It means that `sidebar`, `homepage` and similar keywords should be avoided. 

```css
/* bad */
.home-nav {}
.profile-nav {}

/* good */
.nav-bar {}
.nav-list {}
```

Also, do not style html selectors, instead try to provoid semantic classes:

**Bad**

```html
<div class="profile">
  <span>Name</span>
  <span>Developer</span>
</div>
```

```css
.profile span:first-child {}
.profile span:last-child {}
```

**Good**

```html
<div class="profile">
  <span class="profile-name">Name</span>
  <span class="profile-developer">Developer</span>
</div>
```

```css
.profile .profile-name {}
.profile .profile-developer {}
```

#### Sub classing

If you need to make a variation of a component, try to factorize as much as possible into a common component and create new classes for the variations:

```css
.button {
  border-radius: 2px;
}
.button-primary {
    backgroucolor: #20ff2a;
}
.button-secondary {
    backgroucolor: #4059ff;
}
```

Then you can use them along side:

```html
<a href="#" class="button button-primary">Primary Button</a>
<a href="#" class="button button-secondary">Secondary Button</a>
```

### Views

Page level name-spaces can be helpful for overriding generic components in very specific contexts. It is a last resort, when it doesn't make sense to create a component for it. Page level overrides should be minimal and under a single page level class nest.

```css
.home-page {
  .nav {
    margin-top: 10px;
  }
}
```