+++
author = "the mad geek"
categories = ["getting started", "hugo"]
#tags = ["hugo-templates"]
date = "2020-02-13"
description = "" 
featured = "getting started hugo - 2 of 3/featured.jpg" ## reletave to /static/img/YYYY/MM
featuredalt = "https://pixabay.com/photos/newborn-hand-tiny-finger-4748020/"
featuredpath = "date"
linktitle = "Link Title"
title = "Getting Started Hugo 2 of 3"
type = "post"
draft = false

+++

Picking up from [last time](/blog/2020/01/getting-started-hugo-1-of-3) we have a dirt simple hugo site; applying a simple theme, now we are going to maake a couple of customisations to the template.

Next in this series
- [Getting Started Hugo 3 of 3](/blog/2020/02/getting-started-hugo-3-of-3)

# Wild Assumptions

You have a Google Analytics (GA) account; ready to go.

# Background Information

Partial templates have a specific lookup order (see [the official documentation on Partials](https://gohugo.io/templates/partials/) for details); tldr Hugo will look in the 'local' directory first then the theme directory e.g.

1. `layouts/partials/*<PARTIALNAME>.html`
1. `themes/<THEME>/layouts/partials/*<PARTIALNAME>.html`

# Add GA to Head

So we are going to want to grab the element partial form the template that renders out the `head` element. For _hugo-future-imperfect-slim_ this will be `themes\hugo-future-imperfect-slim\layouts\partials\head.html`. We want to grab that file and copy it to your _local_ partials directory which in our case will be `layouts\partials\head.html`

With a local copy of the file that renders out the `head` element we want to hack that up a little and insert `{{ template "_internal/google_analytics.html" . }}` before the end of the head closure (`</head>`) e.g.

{{< fancybox path="/img/2020/02/getting started hugo - 2 of 3" file="figure1.png" caption="Figure 1" gallery="Post Gallery" >}}

# Update Configuration

Finally you will need to crack open your `config.tomi` file and set your `googleAnalytics` setting to your _UA_ code; if the setting doesnt exist create it

{{< fancybox path="/img/2020/02/getting started hugo - 2 of 3" file="figure2.png" caption="Figure 2" gallery="Post Gallery" >}}

# Closing thoughts

Were barely scratchign the surface here; the main take away is that if you want to hack up some HTML look for a configuration setting that you can toggle in the `tomi` file; failing that identify the partial that is generating the HTML you want to change; make a copy in your local site; and slash and  hack until it looks the way you want it.

Key resources are the Hugo Offical Documentation with the links below being ones that are usefull for the hack and slash.

- [https://gohugo.io/templates/](https://gohugo.io/templates/)
