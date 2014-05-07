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
3. [Thanks](#thanks)



<a name="general">General Principles</a>
-----------

1. Consistent Indentation: 2 spaces per block
2. Consistent spacing before/after braces/colons
3. One selector per line, one rule per line
4. List related properties together
	( width/height, margin/padding etc.)

**Be consistent.** If you’re editing code, take a few minutes to look at the code around you and determine its style. If they use spaces around all their arithmetic operators, you should too. If their comments have little boxes of hash marks around them, make your comments have little boxes of hash marks around them too.

The point of having style guidelines is to have a common vocabulary of coding so people can concentrate on what you’re saying rather than on how you’re saying it. If code you add to a file looks drastically different from the existing code around it, it throws readers out of their rhythm when they go to read it. Avoid this.

<a name="formatting">Formatting</a>
----------

### Spacing and Indentation:
* Indentation should be **two spaces**. If using tabs in a code editor, ensure that tabs are set to two spaces. Each selector should have at least one line break above, this helps to separate selectors and ideas. Properties should immediately follow the selector without a line break, which will aide in putting context to the properties. Remove any trailing whitespaces. Trailing whitespaces are unnecessary and can complicate diffs.
* Opening brace should be on the same line as selector. Closing brace should be on it's own line.


```scss
// bad
h1
{
  padding: 0
  margin: 0
}

// good
h1 {
  margin: 0
  padding: 0
}
```

### Sorting Properties

* All properties should be on their own line.
* Related properties should be placed together.
* List @extend(s) first
* List @include(s) next
* List "regular" styles next
* Nested selectors last.
* Related styles go together.
 Properties should be ordered alphabetically to enable easy lookup. Any includes should go at the top of the propertly list, directly below the selector. Multiple includes should also be ordered alphabetically. 



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
  padding: 0
  line-height: 1.5
  color: #000
}
  
ul {
  color: #666
}
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
body {
  border-top: 1px solid #000
}

article {
  background: #ccc
  padding: 0
}

ul {
  margin: 0;

  li {
    line-height: 1.5em;
  }

  a {
    color: #ccc;
  }
}
```
  
  
Classes, pseduo-classes, and other modifiers should nest inside their selector to maintain readability. The same rules with a line break above apply to these modifiers. Pseduo-class modifiers should go as close to their parent selector as possible. This allows developers to quick see any hover or other states without having to find them later in the file. Class modifiers should also go near their selector whenever possible.  


```sass
// bad
h1.green {
  color: #0085e3
}
  
h1:first-child {
  padding-top: 0
}


// good
h1 {
  &:first-child {
    padding-top: 0
  }
  
  &.green {
    color: #0085e3
  }
```

<a name="thanks">Thanks</a>
======

Inspired by:

* [SMACSS](https://smacss.com/)
* [CSS tricks - SASS Style Guide](http://css-tricks.com/sass-style-guide/)
* [CSS/Sass style guide](https://github.com/isellsoap/css-sass-style-guide)