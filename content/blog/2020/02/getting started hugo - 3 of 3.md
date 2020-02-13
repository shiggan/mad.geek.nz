+++
author = "the mad geek"
categories = ["getting started", "hugo"]
#tags = ["tag a", "tag b"]
date = "2020-02-13"
description = "" 
featured = "getting started hugo - 3 of 3/featured.jpg" ## reletave to /static/img/YYYY/MM
featuredalt = "https://pixabay.com/photos/newborn-hand-tiny-finger-4748020/"
featuredpath = "date"
linktitle = "Link Title"
title = "Getting Started Hugo 3 of 3"
type = "post"
draft = false

+++


Picking up from [last time](/blog/2020/02/getting-started-hugo-2-of-3) we have a site that runs fine locally; now its time to throw it up on github; there are however a couple of gotchas.

Github Pages likes to serve up a branch; and hugo likes to publish to a folder. Thanks to a git trick below we will be able to do both.

So we have our demo site we have been working on; time to put that to a side for now (you can set it on fire later) we are going to step through the process to get a hugo site served up and running on Github Pages.

# Stuff in Git

Create a new repository on GitHub; for this example I will use this site as a reference `mad.geek.nz`.

Create a folder on your local computer; open up a command line in that local folder and:

`git clone https://github.com/shiggan/mad.geek.nz.git .`

The dot (`.`, period) at the end of the command tells it to clone directly in to the current folder without making any new directories.

Thanks to a magic incarnation we will apply for git; The master branch will store the 'built' static website; while for the source-code we will throw that on a second branch; lets call that `source`; now is for some incarnations.

Push in a change to the master branch.

`
git commit --allow-empty -m "first on master"
git push origin master
`

Push in a change to the source branch.

`
git commit --allow-empty -m "first on source"
git push origin source
`

Now the magic incarnation that allows you to have a mixed of branches checked out at the same time. This will let us build the site in the 'public' folder, which is pushed in to the master `master`.

`
git worktree add -B master public origin/master
`
> important : this worktree trick is something that is applied to your `local` repository only; it doesnt get pushed anywhere so if you ever need to pull down your site again you will need to pull out the sacred candles and chant the magical incarnation again in order to make this work.

Because no amount of text can describe just how cool this is; here is what this achieves.

{{< fancybox path="/img/2020/02/getting started hugo - 3 of 3" file="figure1.png" caption="Figure 1" gallery="Post Gallery" >}}

With all of the standard setup done; you can either copy over what you did in steps 1, and 2 or you can start fresh by initalizing a new hugo site in the `source` branch


```
git checkout source
hugo new site . --force
```

# Stuff in GitHub

Hit the Options page for your GitHub Project

{{< fancybox path="/img/2020/02/getting started hugo - 3 of 3" file="figure2.png" caption="Figure 2" gallery="Post Gallery" >}}

Scroll down the options page untill you get to the **GitHub Pages** section

{{< fancybox path="/img/2020/02/getting started hugo - 3 of 3" file="figure3.png" caption="Figure 3" gallery="Post Gallery" >}}

1. Set this to `master`
1. Dont actually set this here.
   By filling this in GitHub will drop a `CNAME` file in the `master` branch; which is not what we want. Instead if you want a custom domain name all you need to do is drop a CNAME file in your `static` folder over on your `source` branch; the only thing the file needs to contain is your custom domain name e.g.
   `mad.geek.nz`
   {{< fancybox path="/img/2020/02/getting started hugo - 3 of 3" file="figure4.png" caption="Figure 4" gallery="Post Gallery" >}}
1. **Turn this On** its free, its from [Lets Encrypt](https://letsencrypt.org/)

# Publish Script

Somewhere on the interwebs I sole a script named `build-publish.cmd`; it wraps up the steps necessary for generating a new version of the site; and pushing it into GitHub; it will even generate a non suck log message which is a bit of a bonus.


```cmd
# generate a less suck commit message
for /f "skip=1" %%x in ('wmic os get localdatetime') do if not defined MyDate set MyDate=%%x
set today=%MyDate:~0,4%-%MyDate:~4,2%-%MyDate:~6,2%
set commitMessage="site rebuild: %today%"
echo %commitMessage%

# burn the old thing down
echo "Removing the old website"
pushd public
git rm -rf *
popd

# build the new thing
echo "Building the website"
hugo

# publish the new thing
echo "Pushing the updated \`public\` folder to the \`master\` branch"
pushd public
git add *
git commit -m %commitMessage%
popd
git push origin master
```

