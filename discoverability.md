---
title: Discoverability in Codebase
date: 2022-08-04
layout: ../../layouts/RequestResponse.astro
---
# Discoverability in Codebase
## What is it?
Code discoverability is how easy it is for a developer to find existing functionalities and dependent functionalities.

## How to rate discoverability
- How easy is to traverse indirections?
- How declarative is the business logic?
	- Declarative code describes "what" as opposed to imperative code which describes "how". It describes the code intent closer to business logic.

## How to achieve it?
### Code Browsers
Code browsers index all the primitive components of codebase. It could be Classes or functions or any abstraction provided by the language.
	Some of the best code browsers I've used are the
#### Pharo's system browser
![](https://files.pharo.org/web-images/carousel/navigation.gif)
		*Credit - https://files.pharo.org/*
#### Eclipse's Java browsing perspective
![](https://querix.com/go/beginner/Content/Resources/Images/05_workbench/01_ls/02_interface/01_perspectives/java/java_browsing_perspective_00_thumb_600_0.png)
		*Credit https://querix.com/*
#### Visual Studio's object browser
![](https://matthiasfriedrich.gallerycdn.vsassets.io/extensions/matthiasfriedrich/visualbasictoolsforvisualstudio/1.6.6/1505850909956/151077/1/object-browser.png)
		*Credit - https://matthiasfriedrich.gallerycdn.vsassets.io*
Edit: 19/02/2023

### Outliners
#### VSCode's Outline view
I heavily use the outline view to have a birds eye on the file although its not as beneficial in React

### Regex
Good'ol Grep/Ripgrep REGEX RULES!!!!!!
Regex would be my last option along with data wrangling.

### Tag browser
Tags generated by `ctags` can be used as well to navigate the codebase.


### Intuitive interfaces
For functional programming It would be sensible function signatures like [Hoogle](https://hoogle.haskell.org/).

For OO It would be Interfaces. I believe you could search classes by receives and sends clause. Pharo's supports this.

Here's a [thread](https://news.ycombinator.com/item?id=32365660) where I asked developers how they go about getting comfortable with a new codebase.
