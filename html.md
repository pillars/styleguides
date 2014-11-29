# HTML Style Guide

This guide is inspired by those guides:

- http://csswizardry.com/2012/04/my-html-css-coding-style/
- http://google-styleguide.googlecode.com/svn/trunk/htmlcssguide.xml

### Document type

HTML5:

```
<!DOCTYPE html>
```

### Case

Lowercase attributes:

```html
<!-- bad -->
<a HREF="#"></a>

<!-- good -->
<a href="#"></a>
```

### Comment on closing tag

```
<div class=content>

    ...

    <div class=carousel>
    ...
    </div><!-- /carousel -->

    ...

</div><!-- /content -->
```

### Namespaced fragment identifiers

I came up with this idea to namespace fragment identifiers last year. It’s basically, I think, a nice way to add a little more meaning to your fragment identifiers and give a little bit more of a clue as to what they actually link to.

http://csswizardry.com/2011/06/namespacing-fragment-identifiers/

```
<a href=#section:fragment-identifiers>Fragment identifiers</a>

...

<div id=section:fragment-identifiers>...</div>
```

### URLS

Do not set the protocol on URLs:

```html
<!-- bad -->
<script src="http://www.exemple.com"></script>

<!-- good -->
<script src="//www.exemple.com"></script>
```

### Order

For consistency, HTML attributes should be written in the following order:

- src, for, type, href
- id, name
- class
- data-*
- title, alt
- aria-*, role

For example:

```html
<nav class="main-nav" role="navigation">
    <a class="btn" id="menu-toggle" data-toggle="dropdown" href="#" role="button">
    <input id="first-name" name="first-name" type="text" />
</nav>
```
