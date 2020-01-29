+++
author = "the mad geek"
categories = ["getting started", "hugo"]
#tags = ["choco"]
date = "2020-01-25"
description = "This will be the first post in a short series about getting started with hugo. By the end of this you will have a _dead simple_ proof of concept site which we will build on in later posts." 
featured = "getting started hugo - 1 of 3/featured.jpg" ## reletave to /static/img/YYYY/MM
featuredalt = ""
featuredpath = "date"
linktitle = "Link Title"
title = "Getting Started Hugo 1 of 3"
type = "post"
draft = false

+++

This will be the first post in a short series about getting started with hugo. By the end of this you will have a _dead simple_ proof of concept site which we will build on in later posts.


Static site generators such as Hugo are great for sites that don’t have any particular reason for dynamic (database driven) content; or for simple sites where the 'dynamic' content you do have rarely changes.

For my site, using a static site generator is ideal as the content itself can be hosted on platforms such as Github Pages Azure Blob Storage; and is relatively straight forward to add content. I don’t want to deal with things like frameworks, databases, security, all I want to do is serve up hypertext and css so out of pure laziness and an endless number of *my own blogging-frameworks* (all of which will never reach the light of github) because lets face it KISS and YAGNI.

This site is hosted on Github; mostly known for the hosting of Git Repositories, I wanted something simple which automated the publishing of changes (this is where Github Pages comes in), easily configurable with a custom domain and preferably a ssl free certificate, so here we go.

## Setup on Windows via Chocklaty

There are lots of ways to get Hugo setup; my preference is to use Chocklaty.

[Get Chocklaty installed](https://chocolatey.org/install) : *or just run this random script form the interwebs*
  
```
"%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe" _ 
  -NoProfile _
  -InputFormat None _ 
  -ExecutionPolicy Bypass _ 
  -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" _
    && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"
```

[Install Hugo](https://gohugo.io/getting-started/installing/#chocolatey-windows)

```
choco install hugo -confirm
```
Or if you want SASS/SCSS
```
choco install hugo-extended -confirm
```

## My First site

So now the bits are installed its time for hello-world mainly because of the instant gratification.

Pick a [Hugo Theme](https://themes.gohugo.io/), Personally I chose [hugo-future-imperfect-slim](https://github.com/pacollins/hugo-future-imperfect-slim) which we will use in this demo.

Drop into a [Terminal](https://github.com/Microsoft/Terminal), punch in the bits below

```
# new directory
hugo new site instant-gratification
cd instant-gratification

# actually a git repository (hugo modules are git-submodules)
git init

# install chosen theme
git submodule add https://github.com/pacollins/hugo-future-imperfect-slim.git themes/hugo-future-imperfect-slim
git submodule init
git submodule update
```

Copy the bits from .\instant-gratification\themes\hugo-future-imperfect-slim\exampleSite into \instant-gratification


```
# generate the site with the selected theme and serve it
hugo server -t hugo-future-imperfect-slim
```

That last line will run the server; and spin you up on a localhost port; hit that endpoint with your web browser and experience

{{< fancybox path="/img/2020/01/getting started hugo - 1 of 3" file="figure1.png" caption="Figure 1" gallery="Post Gallery" >}}

Next time we will apply some customisations to the site.