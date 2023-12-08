---
title: "How to contribute"
date: 2022-10-22T18:53:54+02:00
draft: false
weight: 40
chapter: true
math: true
---
# How to contribute

#### Here we want to help you to further improve this wiki by giving some guidelines and inspiration how to do things (e.g. display pictures, have consistent headings and so on).

## Create a new page

Often you have to edit the wiki and often you have to add new pages. It is fairly simple to add new pages in Hugo. There are basically two ways to do it:

### The better way

The better way is to go to the console while you are in the right directory (CrosForge_Web) and type this command: `hugo new folder/_index.md`

With this you create a new folder and in this new folder a site with the name "_index.md". The "_index.md" page is _always_ the first page you land on when you click on the page name on the sidebar. 

Why is this the better way? This is easy, because while doing this you do not have to implement the necessary front matter at the top of the document. For this document the front matter looks like this: 

```yml 
---
title: "How to contribute"
date: 2022-10-22T18:53:54+02:00
draft: false
weight: 30
chapter: true
math: true
---
```
This website always uses yaml as file type in the front matter. It is also possible to use toml or json. You can find more information [here](https://gohugo.io/content-management/front-matter/).

If you want to learn more about the _new_ command in hugo, you can do that [here](https://gohugo.io/commands/hugo_new/).

### The ugly way 

Sometimes it is just easier to create a new page in your IDE and then implement the front matter. This is possible but a bit risky. You could forget the front matter. 

## Various types of headlines

To stay uniform we want to have consistent headings. Therefore, headlines for bigger topics on one site should be _h2_-Headings (written with two #). This should look something like this: 
```md
## This is a heading
```
For every subheading just add one #.
```md

## This is a heading

### This is a subheading

#### This is a subsubheading

```
Please also leave one blank line before and after every heading!

## Title of a page

A page always needs a title which is displayed at the top. Below that it is always good to write a brief summary of what you want to achieve with that page. 

This can be done with four hashtags:

```md
#### Here we want to help you to further improve this wiki by giving some guidelines and inspiration how to do things (e.g. display pictures, have consistent headings and so on).
```

Below that you can start with a _h2_-heading.



## Chapters
The index file (`_index.md`) should always be a chapter. You can do that by changing the front matter:
```yml 
---
...
chapter: true
---
```

There will be no title displayed. To change that use _h1_-headings (with one #). There will be automaticly a horizontal line displayed.
