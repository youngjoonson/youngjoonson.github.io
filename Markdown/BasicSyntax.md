# Mastering Markdown

## Examples

### Text

It's very easy to make some words **bold** and other words *italic* with Markdown. You can even [link](https://www.google.com) to Google.

### Lists

numbered lists:

1. One
2. Two
3. Three

bullet points:

* start a line with a start
* ha!

Alternatively,

* dashes work just as well
* sub points, put two spaces before dash
  * sub point 1
  * sub point 2
    * sub sub point 1
    * sub sub point 2

### Images

To embed images,

![Image of something, alt message here](https://octodex.github.com/images/yaktocat.png)

### Headers & Quotes

total 6 level headers by '#' to '######'

to quote something or someone, use the '>'

> Coffee. The finest organic uspension ever devised... I beat the Borg with it.
> by Captain Janeway

### Code

For inline code blocks, wrap them in backticks: `int something = 122`. For a longer block of code, indent with four spaces:

    if (isThis){
        return true;
    }

use code fencing which allows for multiple lines without indentation:

```
if (isThis){
    return true;
}
```

To use syntax highlighting, include the language:

```javascript
if(isThis){
    return ture;
}
```

### Extras

Add checkbox like to do list:

* [x] hahaha
* [ ] hohoho

strikethrough text ~~like this~~, wrapping with two tildes
