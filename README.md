SCSS Style Guide
============================

*A guide to developing robust stylesheets in SCSS - based on [SMACSS](https://smacss.com)*

Core goals of SMACSS: 

* Increase the semantic value of a section of html and content
* Decrease the expectation of a specific html structure

Table of Contents
-----------------

1. [General Principles](#general)
2. [Formatting](#formatting)
3. [Organization](#organizing)
	1. [Base Rules](#organizing-base)
	2. [Modules](#organizing-modules)
	3. [States](#organizing-states)
	4. [Pages](#organizing-pages)
4. [Thanks](#thanks)



<a name="general">General Principles</a>
-----------

1. Consistent Indentation: 2 spaces per block
2. Consistent spacing before/after braces/colons
3. One selector per line, one rule per line
4. List related properties together
	( width/height, margin/padding etc.)

###Be consistent.

If you’re editing code, take a few minutes to look at the code around you and determine its style. If they use spaces around all their arithmetic operators, you should too. If their comments have little boxes of hash marks around them, make your comments have little boxes of hash marks around them too.

The point of having style guidelines is to have a common vocabulary of coding so people can concentrate on what you’re saying rather than on how you’re saying it. If code you add to a file looks drastically different from the existing code around it, it throws readers out of their rhythm when they go to read it. Avoid this.

### Use Variables

Convert all colors, common numbers, and numbers with meaning to variables. They are easier to understand and significantly improve readibility.

```scss
// bad
.header { background: #b2b8bc; }

// good
.header { background: $grayblue; }
```

<a name="formatting">Formatting</a>
----------

### Spacing and Indentation:
* Indentation should be **two spaces**. If using tabs in a code editor, ensure that tabs are set to two spaces. Each selector should have at least one line break above, this helps to separate selectors and ideas. Properties should immediately follow the selector without a line break, which will aide in putting context to the properties. Remove any trailing whitespaces. Trailing whitespaces are unnecessary and can complicate diffs.
* Opening brace should be on the same line as selector. Closing brace should be on it's own line.
* If a selector has only one property, keep the selector and property on a single line. This makes it easier to quickly scan a page of SCSS and improves readability.


```scss
// bad
h1
{
  padding: 0;
  margin: 0;
  font-size: 24px;
}

h2 { 
font-size: 18px;
}

h3 { 
font-size: 14px;
}

// good
h1 {
  margin: 0;
  padding: 0;
  font-size: 24px;
}

h2 { font-size: 18px; }
h3 { font-size: 14px; }
```

### Sorting Properties

* All properties should be on their own line.
* List @extend(s) first
* List @include(s) next
* List "regular" styles next
* Classes, pseudo-classes and other modifiers are next.
* Nested selectors last.
	* Layout styles first (positioning, width, heigh, margins etc.)
	* Text styles next (font, line-height)
	* Other styles last
* Related properties should be grouped together. 


```scss
.ul {
  @extend list;
  @include clear;
  width: 200px;
  height: 50px;
  min-width: 50%;
  max-width: 100%;
  color: #000;

  li {
    font-size: 16px;
    font-weight: bold;
    line-height: 1.5em;
    list-style-type: disc;
  }

  a {
    text-decoration: none;
    color: #ccc;
  }
}

```

### Chaining

When possible, if multiple methods have the same styles or very similar styles then chain them together using commas. If two selectors have very similar styles with one or two differences, chain them together with the first selector's styles, then after set the minor changes to the second selector. 

```scss

ol, ul {
  padding: 0;
  line-height: 1.5;
  color: #000;
}
  
ul { color: #666; }
```

### Nesting

Element selector nesting should only be used when necessary. Nesting should be two spaces, and any selectors should have a line break before them.

**Maximum nesting: three levels deep.** Chances are, if you're deeper than that, you're writing a crappy selector. Crappy in that it's too reliant on HTML structure (fragile), overly specific (too powerful), and not very reusable (not useful). It's also on the edge of being difficult to understand.


```sass
// bad
body {
  border-top: 1px solid #000;

  > article {
    background: #ccc;
    padding: 0;
  }

  ul {
    margin: 0;

    li {
      line-height: 1.5em;

      a {
        color: #ccc;
      }
    }
  }

}



// good
body { border-top: 1px solid #000; }

article {
  background: #ccc;
  padding: 0;
}

ul {
  margin: 0;

  li { line-height: 1.5em; }
  a { color: #ccc; }
}
```
  
  
Classes, pseduo-classes, and other modifiers should nest inside their selector to maintain readability. The same rules with a line break above apply to these modifiers. Pseduo-class modifiers should go as close to their parent selector as possible. This allows developers to quick see any hover or other states without having to find them later in the file. Class modifiers should also go near their selector whenever possible.  


```scss
// bad
h1.green {
  color: #0085e3
}
  
h1:first-child {
  padding-top: 0
}


// good
h1 {
  &:first-child { padding-top: 0;  }
  &.green { color: #0085e3; }
}
```

### Media Queries

Media Queries should be named and nested whenever possible. 

```scss
// bad

.content {
  width: 60%; 
}

@include screen and (max-width: 800px) {
  .content { 
    width: 80%; 
  }
}

@include screen and (max-width: 540px) {
  .content { 
    width: 100%; 
  }
}


// good 

.content {
  width: 60%;
	
  @include breakpoint($screen-tablet) {
    width: 80%;
  }	

  @include breakpoint($screen-mobile) {
    width: 100%;
  }	
}
```

### Performance

Do not worry about CSS selector performance or file size. The file will be minified and gzipped for production, so duplicate media queries or selectors will not impact file size. 

CSS selector performance is extremely fast in all modern browsers, so CSS parsing has virtually no impact on page performance. So, focus on clean and readable CSS and *[do not sacrifice semantics or maintainability for efficient CSS.](http://css-tricks.com/efficiently-rendering-css/)* [For most web sites, the possible performance gains from optimizing CSS selectors will be small, and are not worth the costs.](http://www.stevesouders.com/blog/2009/03/10/performance-impact-of-css-selectors/)

<a name="structure">File & Folder Structure</a>
======

Style rules are organized into these basic categories:

* Base: defaults. 
* Layout: basic grid layout, site/application layout
* Module: reusable components that will be used in multiple locations.
* State: describe how things look in a particular state: hidden, displayed, active, inactive, disabled etc.
* Page: rules that only apply to specific sections or pages of the site/application.

Naming convention:

* Base: body, h1, p
* State: "is-" prefix. ( .is-active , .is-hidden)
* Module: Modules use the module name (.audio-player)
* All ids & classes that use multiple words use a hypen. (.audio-player, .user-access)

### <a name="organizing-base">Base Rules (/base)</a>

Applied directly to selectors. No specific classes or ids. Reset or Normalize are an example of base styles.

```scss
a {
	color: blue;
	text-decoration: dotted;
	
	&:hover, 
	&:focus { color: green; }
	
	&:visited { color: gray; }
}
```

In addition, these components should also be included in the "base/" directory.

* /grid.scss: The responsive grid that applied to all components 
* /colors.scss: Basic color variables. All colors that are used in the site should be included here and should have descriptive names whenever possibe. Example: instead of $green or $purple, use $primary-color, $secondary-color etc.
* /typography.scss: Includes basic typography
* /forms.scss: basic layout for forms: labels, inputs, fieldsets, errors, hints etc.

### <a name="organizing-modules">Modules (/modules)</a>

Modules sit inside layout components or inside other modules. 

* Each module should be designed as a standalone component that can be used on any page in any component. 
* Can be moved to a different section or page without the layout breaking. They should not be dependant on page structure.
* Should be easy to customize. Modules will be customized based on requirements on different pages, so should be written with low specificity.
* Modules generally don't have a width specified, since they are designed to sit within a layout.


<a name="thanks">Thanks</a>
======

Inspired by:

* [SMACSS](https://smacss.com/)
* [CSS tricks](http://css-tricks.com/sass-style-guide/)
* [CSS/Sass style guide](https://github.com/isellsoap/css-sass-style-guide)