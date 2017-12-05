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
