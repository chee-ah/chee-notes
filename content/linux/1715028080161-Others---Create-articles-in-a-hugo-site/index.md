---
title: "Others - Create articles in a hugo site"
date: 2024-05-06
draft: false
description: "Create articles in a hugo site"
tags: ["static", "site", "linux", "hugo", "npm", "go", "golang", "node"]
---
# Create articles in a hugo site

In these notes we'll describe the process of writing and publishing a new article in for this here website.

## Install the blowfish tools

This site is using the static site generator `hugo`, with the theme `blowfish`. This theme comes with a handy little set of command line tools. While hugo itself is written in golang, the blowfish tolls are written in `node js`, so first of all we need to install `npm`, the node package manager on our workstation.

First update and upgrade brew.
```bash
brew update
brew upgrade
```

Then install npm
```bash
brew install npm
```

Once we have installed npm which is a package manager for node js packages, we can now install the blowfish tools, because they're written in node js.
```bash
npm i -g blowfish-tools
```

We can use which, just to confirm that we now have those tools somewhere in our path.
```bash
which blowfish-tools
```

## Use the blowfish tools

We can do a lot of things pertaining to the configuration of our static site with those tools, but the point of this article really is just to describe how to create a new article in the site. So let's do that.

Start to tools. This will launch a little interactive app whith a nice blowfish ascii art.
```bash
blowfish-tools
```

Then we'll need to do the following:
```
> select the option to `Generate a new article`
> select the category of the site where we want the article to be, for this one: `linux`
> choose a name for the article, for this one: 'Others - Notes about the bash shell'
> exit the tools
```

Now from a normal bash shell we can navigate in the directory structure of our site.
```bash
cd chee-notes
ls -la 
cd contents
ls -la 
cd linux
ls -la
```

We'll see that the npm app created a new folder here for us, see the `Notes-about-the-bash-shell`
```bash
ls -l
total 0
drwxr-xr-x@ 4 chee  staff  128 May  6 21:36 1712426513705-LPI---Linux-Essentials---The-Basics
drwxr-xr-x@ 4 chee  staff  128 Apr  6 22:24 1712426557493-LPI---Linux-Essentials---Common-CLI-Tools
drwxr-xr-x@ 4 chee  staff  128 Apr  6 22:24 1712426570291-LPI---Linux-Essentials---Shell-Redirects
drwxr-xr-x@ 5 chee  staff  160 May  6 22:02 1715028080161-Others---Create-articles-in-a-hugo-site
drwxr-xr-x@ 4 chee  staff  128 May  6 22:12 1715029765425-Others---Notes-about-the-bash-shell
```

If we navigate to that new folder, we have files created for us in there as well
```bash
cd 1715029765425-Others---Notes-about-the-bash-shell
tree .
.
├── featured.png
└── index.md
```

The index.md is where our markdown notes are gonna go, and it already has automatically generated markdown metadata at the top, it's basically just yaml key value pairs that describe our document for better `seo` latter on, seo means search engine optimisation, it's just how google finds, and understands the content that is in the page. We can of course, modify or improve that info manually. For instance we can add tags relevant to the content.  

The featured.png file is of course an image, this is image that will show up in the page for our article, the tools have setup a default image for us, but we can change it to whatever we want, as long as it's the same file name, but for us, because it's in the linux section we'll use the penguins, because of course.  

To do that, let's copy the penguins from an older article
```bash
cp ../1712426513705-LPI---Linux-Essentials---The-Basics/featured.png .
```

Double check that we have the right file by opening it in a image reader
```bash
open featured.png
```

## Write Markdown

Cool so we're now done playing with the blowfish tools, to create the skeleton of our article. Now all that's left to do is to put some flesh on it, namely, write the content of the article... Here's a quick `markdown` cheat sheet:

### Code blocks

Code blocks are done like so:
```
```python
while true:
    print("I can write code I swear")
```
```

We can choose different language based on what we're writing in order to choose the correct syntax highlighting lexer
```
```bash
while true; do
    printf "I can write code I swear";
done
```
```

or specify to use no lexer if what we write isn't actually valid code, or if we don't want syntax highlighting 
```
```
- just trying to convey some ideas in a bullet point form here
- yep yep
- yep
```
```

### Bullet points

```
* one
* two
* three
```
```
- one
- two
- three
```
```
1. one
2. itwo
3. three
```

### Checkboxes
```
- [ ] (for unchecked checkbox)
- [x] (for checked checkbox)
```

### Headings
```
# h1
## h2
### h3
#### h4
```

### Bold / Italic

```
**bold**
*italic*
```

There's a lot more we can do with markdown, but it's a good start I think.

## Publishing the article

Now assuming we did everything right and wrote our article in valid markdown, all that's left to do is to publish the article. For this we need to use github.
```bash
git add -u
git status
git add content/linux/1715028080161-Others---Create-articles-in-a-hugo-site/
git add content/linux/1715029765425-Others---Notes-about-the-bash-shell/
```

```bash
```

```bash
```

```bash
```

```bash
```

```bash
```

```bash
```

```bash
```

```bash
```



