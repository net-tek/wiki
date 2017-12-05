<!-- TITLE: Wiki Tips -->
<!-- SUBTITLE: A quick summary of Wiki Tips -->

# Blockquotes
## What is a blockquote
A blockquote is originally used to define a section that is quoted from another source. However, Wiki.js offers many additional options to the blockquote.

## How to define a blockquote
Simply begin your line of text with a **>**

For example, the following markdown text:


```text
> A line of text

> A blockquote with
> 
> multiple lines  
> of text!

> You can even include **styling** in your text, or **icons** :apple:
```

would result in:

> A line of text

> A blockquote with
> 
> multiple lines  
> of text!

> You can even include **styling** in your text, or **icons** :apple:

## Available styling

Various styling options are available:

>Default

>Info
{.is-info}

>Success
{.is-success}

>Warning
{.is-warning}

>Danger
{.is-danger}

Simply add the desired styling class below your blockquote:


```text
> Default

> Info
{.is-info}

> Success
{.is-success}

> Warning
{.is-warning}

> Danger
{.is-danger}
```


# Keyboard shortcuts
## General shortcuts
* **CTRL+S:** Save and view processed page.

## Formatting shortcuts
* **CTRL+B:** Set selected text as bold.
* **CTRL+I:** Set selected text as italic.
* **CTRL+L:** Start a bullet list.
* **CTRL+ALT+L:** Start a numbered list.

# Markdown Syntax
## What is Markdown
Markdown is a lightweight markup language with plain text formatting syntax designed so that it can be converted to HTML and many other formats using a tool by the same name. Markdown is often used to format readme files, for writing messages in online discussion forums, and to create rich text using a plain text editor.

# Basics
## Bold
To make a text in bold, simply put double asterisks `**` before and after the desired text:

`To **boldly** go` -> To **boldly** go

## Strikethrough
To make a text in italic, simply put double tildes ~~ before and after the desired text:

`Something ~~not cool~~` -> Something ~~not cool~~

# Headers
Headers are useful to break a long page into sections. For example, the page you are currently reading has multiple headers and sub-headers:

* What is Markdown
* Basics
	- Bold
	- Italic
	- Strikethrough
* Headers
* etc...

To define a line as a header, simply put a hash sign # at the beginning. The amount of consecutive # you insert defines the level of the header.


```text
# H1
## H2
### H3
#### H4
##### H5
###### H6
```

Example:


```text
# Section A

## Sub-section A.1

### Sub-sub-section A.1.1

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed at sapien at odio fringilla lobortis.

# Section B

Etc...
```

In the above example, Section A and B are top level headers and Section A has a sub-header (A.1), which contains a sub-header (A.1.1).

# Lists
Unordered lists can be defined using a dash at the beginning of every line:

```markdown
- Item 1
- Item 2
- Item 3
```

Ordered lists can be defined using numbers:


```markdown
1. Item
2. Item
3. Item
```

Note that ordered lists are automatically incremented. You can therefore use 1. for all items.

# Links
Links to other page or external webpages are defined using the following syntax:


```markdown
[Internal Link Title](/path/to/page)
[External Link Title](https://www.google.com/)
```

A special icon is automatically displayed next to external links to denote the user will leave the site if clicked.

# Images
Images can be inserted in almost the same way as links. They simply require an extra ! at the beginning:


```markdown
![Image Caption](http://link.to/image.jpg)
```

# Code
## Inline
Inline code provide a quick way to insert `code / commands` without creating a standalone code block. They are enclosed by a single back-tick:


```markdown
`This is an inline code`
```

## Block

If your code requires multiple lines or you need syntax highlighting, it is preferrable to use code blocks. They are enclosed by triple backticks:


````
```
var sample = 'code';

on.multiple(lines) {
    cool();
}
```
````

To add syntax highlighting, simply add the language name right after the opening triple backticks:


````
```js
var sample = 'code';

on.multiple(lines) {
    cool();
}
```
````

# Horizontal Rule

Simply put **three (3)** of the following characters to create an horizontal rule: `***` (asterisks) or `---` (hyphens)
Which results in:

# Footnotes
You can easily add footnote references using the `[^x]` syntax (where `x` is a number or name):


```markdown
Here is a footnote reference,[^1] and another.[^longnote]

[^1]: Here is the footnote.

[^longnote]: Here's one with multiple blocks.

    Subsequent paragraphs are indented to show that they
belong to the previous footnote.
```

You can also inline footnotes directly:


```markdown
Here is an inline note.^[Inlines notes are easier to write, since
you don't have to pick an identifier and move down to type the
note.]
```






