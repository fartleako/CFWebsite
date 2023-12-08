---
title: "Markdown Guide"
date: 2022-10-22T18:53:54+02:00
draft: false
weight: 10
chapter: false
math: true
---

#### Hugo uses markdown files. For this reason it is quite easy to fill the website with knowledge. The [markdown guide](https://www.markdownguide.org/basic-syntax/) should help you to get quickly into the file format.

Below we have also listed the most usefull markdown syntax which we have used to create this site.

## Create links

### To an external site

#### A plain link

This is quite easy. Either way you just paste the url: https://github.com/CrossForge/CrossForge. 

In code this looks like this:
```md
... just paste the url: https://github.com/CrossForge/CrossForge
```

#### A hidden link

The nicer way is to "hide" the link like this: [CrossFoge](https://github.com/CrossForge/CrossForge).
This can be achieved by putting the displayed name into square brackets and the link afterwards into parentheses. In code it looks like this: 
```md 
like this: [CrossFoge](https://github.com/CrossForge/CrossForge).
```

### An internal link

If you want to reference a page on the same website, this is done by stating the relative path like this: 
```md
[First Steps](/basic_concepts/getting_started/)
```
The easiest way is just to copy the link (the url) and state it after the brackets.

## Codeblocks
Till now you may have seen a couple code blocks. They look like this:
```cpp
m_WinWidth = 1280;
m_WinHeight = 720;
```
or like this: `m_WinWidth = 1280`.

The upper one can be created with three "`".
```md
⠀```md - you state your language here (like md for markdown)
 here comes your code
⠀```
```

The inline code block can be created with two "`" between which you write your code.

## Empty or new lines 

New or empty lines like

&nbsp; 

this. Can be added via `&nbsp;` command. 

```md
New or empty lines like

&nbsp; 

this. Can be added via `&nbsp;` command. 
```

## Add pictures and gifs 

Pictures and gifs are added in the same way. First you have to add the according file to the static folder of the framework. Afterwards you can reference this picture in every other file via the relative path. 

The result looks like this. 

```md 
![multiple view ports scene as a gif](/multipleViewPorts.gif)
```
You can find the command to add an image here. 

## Horizontal lines

To seperate certain topics from one another you can use horizontal lines.

---

These can be written as followed:
```md
--- or *** or ___ 
```

Please leave on blank line before and after every horizontal line.

## Tables

In need of a table? See how it's done in the following section.

#### 1) First of all we can use "```|```" to differentiate between columns and secondly we need end rows to designate the header "```|---|```"
```md
| Column1 | Column2 |
|---|---|
```
This results in:

| Column1 | Column2 |
|---|---|

---

#### 2) To add content simply add them like so:
```md
| Column1 | Column2 |
|---|---|
|Content1|Content2|
```

| Column1 | Column2 |
|---|---|
|Content1|Content2|

---

#### 3) One way of centering the whole Column is by placing "```:```" before and after the "```---```" inside of "```|---|```"
```md
| Column1 | Column2 |
|:---:|---|
|Content1|Content2|
```

| Column1 | Column2 |
|:---:|---|
|Content1|Content2|

Here you can learn more about [tables in markdown](https://www.markdownguide.org/extended-syntax/). 