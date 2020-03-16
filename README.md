
# SOT CSS / Sass Styleguide

*forked from [airbnb/css](https://github.com/airbnb/css)*
*A mostly reasonable approach to CSS and Sass*

## Table of Contents

1. [Terminology](#terminology)
    - [Rule Declaration](#rule-declaration)
    - [Selectors](#selectors)
    - [Properties](#properties)
1. [Framework](#framework)
	- [Bootstrap](#bootstrap)
1. [CSS](#css)
    - [Formatting](#formatting)
    - [Comments](#comments)
    - [OOCSS and BEM](#oocss-and-bem)
    - [ID Selectors](#id-selectors)
    - [Nested Selectors](#nested-selectors)
    - [Qualified Selectors](#qualified-selectors)
    - [JavaScript hooks](#javascript-hooks)
    - [Border](#border)
    - [Shorthand](#shorthand)
    - [Flexbox](#flexbox)
    - [Media Queries](#media-queries)
1. [Sass](#sass)
    - [Syntax](#syntax)
    - [Ordering](#ordering-of-property-declarations)
    - [Variables](#variables)
    - [Mixins](#mixins)
    - [Extend directive](#extend-directive)
    - [Nested selectors](#nested-selectors)
1. [License](#license)

## Terminology

### Rule declaration

A “rule declaration” is the name given to a selector (or a group of selectors) with an accompanying group of properties. Here's an example:

```css
.listing {
  font-size: 18px;
  line-height: 1.2;
}
```

### Selectors

In a rule declaration, “selectors” are the bits that determine which elements in the DOM tree will be styled by the defined properties. Selectors can match HTML elements, as well as an element's class, ID, or any of its attributes. Here are some examples of selectors:

```css
.my-element-class {
  /* ... */
}

[aria-hidden] {
  /* ... */
}

[class^="btn-"] {
  /* ... */
}
```

### Properties

Finally, properties are what give the selected elements of a rule declaration their style. Properties are key-value pairs, and a rule declaration can contain one or more property declarations. Property declarations look like this:

```css
/* some selector */ {
  background: #F1F1F1;
  color: #333;
}
```

**[⬆ back to top](#table-of-contents)**

## Framework

A **CSS framework** is a library allowing for easier, more standards-compliant web design using the [Cascading Style Sheets](https://en.wikipedia.org/wiki/Cascading_Style_Sheets "Cascading Style Sheets") language. Most of these frameworks contain at least a [grid](https://en.wikipedia.org/wiki/Grid_(graphic_design) "Grid (graphic design)"). More functional frameworks also come with more features and additional [JavaScript](https://en.wikipedia.org/wiki/JavaScript "JavaScript") based functions, but are mostly design oriented and focused around interactive UI patterns. This detail differentiates CSS frameworks from other [JavaScript frameworks](https://en.wikipedia.org/wiki/Comparison_of_JavaScript_frameworks "Comparison of JavaScript frameworks"). (*Source:* [Wikipedia](https://en.wikipedia.org/wiki/CSS_framework))

We utilize the Bootstrap framework on SOT pages.

### If using Bootstrap

* When creating new pages, use the latest Bootstrap version (currently 4.x)
	- If a page is still utilizing Bootstrap 3, it does not need to be updated.
* Utilize the large collection of helper/utility classes available before introducing custom class names or re-declaring the same CSS property declarations over and over again. (Refer to docs: [Bootstrap Utilities](https://getbootstrap.com/docs/4.4/utilities/borders/))
  - Prefer bootstrap abstraction over re-declaring the same properties in CSS over and over again. See Flexbox example below.
* Only use the grid structure if you need it. Avoid unnecessary HTML markup.
* Avoid HTML duplication. If using Bootstrap 4, since it is all based on flexbox you can re-arrange the order of elements using helper classes (or the CSS `order` property).

**Examples**
```html
<!-- Adjusting padding or margin on a column element that has no unique class name -->
<div class="col-sm-6 mb-0 pl-2"></div>

<!-- Applying flexbox properties without constant duplication in CSS file -->
<div class="d-flex align-items-center justify-content-center"></div>

<!-- Conditionally display content based on viewport breakpoint -->
<div class="d-none d-md-block"></div>
```

## CSS

### Formatting

* Use soft tabs (2 spaces) for indentation.
* Prefer dashes over camelCasing in class names.
  - Underscores are okay if you are using BEM (see [OOCSS and BEM](#oocss-and-bem) below).
* Do not use ID selectors.
* When using multiple selectors in a rule declaration, give each selector its own line.
* Put a space before the opening brace `{` in rule declarations.
* In properties, put a space after, but not before, the `:` character.
* Put closing braces `}` of rule declarations on a new line.
* Put blank lines between rule declarations.

**Bad**

```css
.avatar{
    border-radius:50%;
    border:2px solid white; }
.no, .nope, .not_good {
    // ...
}
#lol-no {
  // ...
}
```

**Good**

```css
.avatar {
  border-radius: 50%;
  border: 2px solid white;
}

.one,
.selector,
.per-line {
  // ...
}
```

### Comments

* Prefer line comments (`//` in Sass-land) to block comments.
* Prefer comments on their own line. Avoid end-of-line comments.
* Write detailed comments for code that isn't self-documenting:
  - Uses of z-index
  - Compatibility or browser-specific hacks

### OOCSS and BEM

We encourage some combination of OOCSS and BEM for these reasons:

  * It helps create clear, strict relationships between CSS and HTML
  * It helps us create reusable, composable components
  * It allows for less nesting and lower specificity
  * It helps in building scalable stylesheets

**OOCSS**, or “Object Oriented CSS”, is an approach for writing CSS that encourages you to think about your stylesheets as a collection of “objects”: reusable, repeatable snippets that can be used independently throughout a website.

  * Nicole Sullivan's [OOCSS wiki](https://github.com/stubbornella/oocss/wiki)
  * Smashing Magazine's [Introduction to OOCSS](http://www.smashingmagazine.com/2011/12/12/an-introduction-to-object-oriented-css-oocss/)

  **OOCSS follows two main "rules"**

  1. Separation of structure and skin. Structure consisting of instructions for *how things are laid out*, and skin defines *what the layout looks like*.
    - Structure refers to things that are "invisible" to the user. These properties include: Height, Width, Margins, Paddings, Overflow.
    - Skin refers to the visual properties, such as colors, fonts, shadows, gradients.

  ```css
  /* Structure */
  .sidebar-card {
    width: 100%;
    padding: 10px 5px;
  }
  .btn {
    width: 200px;
    padding: 5px 10px;
    margin-bottom: 10px;
  }

  /* Skinning */
  .recent-posts {
    font-family: Helvetica, Arial, sans-serif;
    color: #2b2b2b;
    font-size: 19px;
  }
  .btn-primary {
    background-color: #FFF;
    color: #333;
  }

  /* 
  HTML:
  <div class"sidebar-card recent-posts">
    <button class="btn btn-primary"></button>
  </div> 
  */
  ```

  In the example above, if we moved the `.recent-posts` element to a different area of the page, such as the footer, it would look and feel the same but take on the positioning set by the footer.

  2. Separation of container and content.
    - As a general rule, styles should never be scoped to particular containers. Otherwise, you'll be unable to reuse them without applying overrides. For example, you can use the class combination `btn-small btn-red` to ensure that you see a small, red button regardless of the container it appears in so long as the structure class `btn-small` and skin class `btn-red` are written independent of a container.

  **Bad**

  ```css
  .sidebar {
    padding: 2px;
    left: 0;
    margin: 3px;
    position: absolute;
    width: 140px;
  }


  .sidebar .list {
    margin: 3px;
  }


  .sidebar .list .list-header {
    font-size: 16px;
    color: red;
  }


  .sidebar .list .list-body {
    font-size: 12px;
    color: #FFF;
    background-color: red;
  }
  ```

  **Good**

  ```css
  .sidebar {
    padding: 2px;
    left: 0;
    margin: 3px;
    position: absolute;
    width: 140px;
  }

  .list {
    margin: 3px;
  }

  .list-header {
    font-size: 16px;
    color: red
  }

  .list-body {
    font-size: 12px;
    color: #FFF;
    background-color: red;
  }
  ```

**BEM**, or “Block-Element-Modifier”, is a _naming convention_ for classes in HTML and CSS. It was originally developed by Yandex with large codebases and scalability in mind, and can serve as a solid set of guidelines for implementing OOCSS.

  * CSS Trick's [BEM 101](https://css-tricks.com/bem-101/)
  * Harry Roberts' [introduction to BEM](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/)

**BEM Example**

```html
<article class="listing-card listing-card--featured">

  <h1 class="listing-card__title">Adorable 2BR in the sunny Mission</h1>

  <div class="listing-card__content">
    <p>Vestibulum id ligula porta felis euismod semper.</p>
  </div>

</article>

```

```css
/* ListingCard.css */
.listing-card { }
.listing-card--featured { }
.listing-card__title { }
.listing-card__content { }
```

  * `.listing-card` is the “block” and represents the higher-level component
  * `.listing-card__title` is an “element” and represents a descendant of `.listing-card` that helps compose the block as a whole.
  * `.listing-card--featured` is a “modifier” and represents a different state or variation on the `.listing-card` block.

### ID selectors

While it is possible to select elements by ID in CSS, it should generally be considered an anti-pattern. ID selectors introduce an unnecessarily high level of [specificity](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity) to your rule declarations, and they are not reusable.

For more on this subject, read [CSS Wizardry's article](http://csswizardry.com/2014/07/hacks-for-dealing-with-specificity/) on dealing with specificity.

If an ID is declared in HTML then it should imply that it is being used in a JS file somewhere. If it is not being used in a JS file, then the ID should not exist. Class names should be preferred over IDs for use in JS, but there may be times when it is necessary to ensure that there is only one element that the JS is targeting, in which case we would use an ID selector.

### Nested selectors

Do not nest selectors unnecessarily. If `.header-nav {}` will work, never use `.header .header-nav {}`; to do so will literally double the specificity of the selector without any benefit.

### Qualified selectors

Do not qualify selectors unless you have a compelling reason to do so. If `.nav {}` will work, do not use `ul.nav {}`; to do so would not only limit the places you can use the `.nav` class, but it also increases the specificity of the selector, again, with no real gain.

**Make heavy use of classes** because they are the ideal selector: low specificity (or rather, all classes have the same specificity, so you have a level playing field), great portability, and high reusability.

### JavaScript hooks

Avoid binding to the same class in both your CSS and JavaScript. Conflating the two often leads to, at a minimum, time wasted during refactoring when a developer must cross-reference each class they are changing, and at its worst, developers being afraid to make changes for fear of breaking functionality.

We recommend creating JavaScript-specific classes to bind to, prefixed with `.js-`:

```html
<button class="btn btn-primary js-request-to-book">Request to Book</button>
```

### Border

Use `0` instead of `none` to specify that a style has no border.

**Bad**

```css
.foo {
  border: none;
}
```

**Good**

```css
.foo {
  border: 0;
}
```

### Shorthand

Use shorthand when applicable.

**Hex Values**:  `#FFF` instead of `#FFFFFF` when all of the values are the same.

**Margin/Padding**: `top | right | bottom | left` or `top/bottom | right/left` or `top | right/left | bottom` or `top/right/bottom/left`

**Border**: `border-width | border-style | color` and `border-top | border-right | border-bottom | border-left`

### Flexbox

Flexbox is especially useful for creating dynamic layouts such as:
* Vertical and Horizontal alignment and centering
* Dynamic scaling (match heights)
* Re-ordering elements within a container

* [A guide to flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox)
* [Simple, free 20 video course on Flexbox](https://flexbox.io/)
* [Fun way to learn flexbox](https://flexboxfroggy.com/)

### Media Queries

* A mobile first approach is preferred, but a desktop first approach is still acceptable.
  - Mobile first meaning the base styles are for mobile and the use of `min-width` media queries adjust for the larger views.
* All media queries should follow the bootstrap breakpoints unless you need to fine tune certain elements in which case you may introduce new breakpoints, but these should be used sparingly.
* Depending on if you're using Bootstrap 3 or 4, the breakpoints may be a little bit different.

```css
/* BOOTSTRAP 4 */
/* Extra small devices (portrait phones, less than 576px) */
/* No media query for `xs` since this is the default in Bootstrap */

/* Small devices (landscape phones, 576px and up) */
@media (min-width: 576px) { ... }

/* Medium devices (tablets, 768px and up) */
@media (min-width: 768px) { ... }

/* Large devices (desktops, 992px and up) */
@media (min-width: 992px) { ... }

/* Extra large devices (large desktops, 1200px and up) */
@media (min-width: 1200px) { ... }
```

```css
/* BOOTSTRAP 3 */
/* Extra small devices (phones, less than 768px) */
/* No media query since this is the default in Bootstrap */

/* Small devices (tablets, 768px and up) */
@media (min-width: 768px) { ... }

/* Medium devices (desktops, 992px and up) */
@media (min-width: 992px) { ... }

/* Large devices (large desktops, 1200px and up) */
@media (min-width: 1200px) { ... }
```


**[⬆ back to top](#table-of-contents)**

## Sass

### Syntax

* Use the `.scss` syntax, never the original `.sass` syntax
* Order your regular CSS and `@include` declarations logically (see below)

### Ordering of property declarations

1. Property declarations

    List all standard property declarations, anything that isn't an `@include` or a nested selector.

    ```scss
    .btn-green {
      background: green;
      font-weight: bold;
      // ...
    }
    ```

2. `@include` declarations

    Grouping `@include`s at the end makes it easier to read the entire selector.

    ```scss
    .btn-green {
      background: green;
      font-weight: bold;
      @include transition(background 0.5s ease);
      // ...
    }
    ```

3. Nested selectors

    Nested selectors, _if necessary_, go last, and nothing goes after them. Add whitespace between your rule declarations and nested selectors, as well as between adjacent nested selectors. Apply the same guidelines as above to your nested selectors.

    ```scss
    .btn {
      background: green;
      font-weight: bold;
      @include transition(background 0.5s ease);

      .icon {
        margin-right: 10px;
      }
    }
    ```

### Variables

Prefer dash-cased variable names (e.g. `$my-variable`) over camelCased or snake_cased variable names. It is acceptable to prefix variable names that are intended to be used only within the same file with an underscore (e.g. `$_my-variable`).

### Mixins

Mixins should be used to DRY up your code, add clarity, or abstract complexity--in much the same way as well-named functions. Mixins that accept no arguments can be useful for this, but note that if you are not compressing your payload (e.g. gzip), this may contribute to unnecessary code duplication in the resulting styles.

### Extend directive

`@extend` should be avoided because it has unintuitive and potentially dangerous behavior, especially when used with nested selectors. Even extending top-level placeholder selectors can cause problems if the order of selectors ends up changing later (e.g. if they are in other files and the order the files are loaded shifts). Gzipping should handle most of the savings you would have gained by using `@extend`, and you can DRY up your stylesheets nicely with mixins.

### Nested selectors

**Do not nest selectors more than three levels deep!**

```scss
.page-container {
  .content {
    .profile {
      // STOP!
    }
  }
}
```

When selectors become this long, you're likely writing CSS that is:

* Strongly coupled to the HTML (fragile) *—OR—*
* Overly specific (powerful) *—OR—*
* Not reusable


Again: **never nest ID selectors!**

If you must use an ID selector in the first place (and you should really try not to), they should never be nested. If you find yourself doing this, you need to revisit your markup, or figure out why such strong specificity is needed. If you are writing well formed HTML and CSS, you should **never** need to do this.

## License

(The MIT License)

Copyright (c) 2020 BeenVerfied, Inc.

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the 'Software'), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

**[⬆ back to top](#table-of-contents)**