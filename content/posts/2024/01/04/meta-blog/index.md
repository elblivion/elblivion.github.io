+++
title = 'Meta Blog'
date = 2024-01-04T17:36:37+01:00
draft = false
+++

_In this meta-post, I describe how I returned to my abandoned blog and how I set it up._

After many years without posting I decided to revive and revamp this blog. A big factor in this was the trouble 
a friend of mine had with their LinkedIn account; it made me realise that large social media companies 
essentially own and control the content you put on them, so "LinkedIn Posts" or Medium or what have you are
no replacement for having control over what you write.

Also I wanted to get the feel for the complexity required to run a site like this in 2024, vs the ease of 
Linkedin or Medium, which I have not used but I image provide a reasonable in-browser editing and management
experience. The TL;DR for that is that, apart from the usual issues around managing DNS and SSL, this is
still something that seems challenging for non-computer people, but perhaps that's also related to the 
flexibility of the static site generator I'm using.

## Where to host 🌍️

Initially I considered hosting on Vercel, since I have been hearing a lot about that. But a static site isn't 
really the right use case, so I decided to stick with Hugo + Github Pages and simply:

1. Add a custom domain (in case I ever want to move the site)
1. Add SSL (because, you know, it's 2024)
1. Add an automated workflow for publishing on commit (see the previous line)
1. Upgrade Hugo, tweak the theme and and structure a bit

## Custom domain 💅️

Actually picking one was haaaarrrrd. (Naming things usually are). I
haven't run a website for ages, there are gazillions of TLDs now that I'd
never even considered. In the end I chose a very old skool thing (`<my
old IRC nick>.net`) and that will have to do for now. Since I intend to
own the domains this may run on, I can always set up redirects later.

As to the registrar: I definitely didn't want to use GoDaddy, and Amazon
Route53 (the other registrar I have used at work) seemed overkill if I
wasn't going to deploy on AWS, plus it has a lot less TLDs on offer. After
reading some comparisons, especially [this one on the Pragmatic Programmer
blog](https://blog.pragmaticengineer.com/domain-registrars-which-developers-recommend/),
I picked [Porkbun](https://porkbun.com/) because of good reviews and
cute pigs.

## SSL 🔒️

This turned out to be very simple nowadays on Github Pages, even with custom domains; you just click a checkbox
and eventually a Let's Encrypt certificate is deployed. I did enable this before the domain was live (for the
default github.io subdomain) and ended up having to disable and re-enable custom domain until eventually the
DNS & certificate validation dance worked.


## Automated workflow 🏗️

This was pretty simple; I followed the [instructions on the Hugo website](https://gohugo.io/hosting-and-deployment/hosting-on-github/).
No messing around with credentials or anything, apart from changing the values indicated (e.g. branch name) it just worked once
the custom domain was set up (that is used as an input for the workflow steps).

My previous blog was on the `master` branch; I deployed this new config on `main` and later made it the default. This also 
helped when updating the structure of the site, see below.

## Hugo 📝️

I probably picked [Hugo](https://gohugo.io/) simply because it was written in Go. Not that I thought I'd ever 
contribute. My existing site - untouched for over 7 years - was made with whatever was the old version of Hugo that
existed back then. Since I decided to use a new default branch, I chose to simply create a new site and copy two
extremely old and pointless posts over. I never customised too many things or created much content anyway.

Now in 2024 adding a theme was pretty straightforward, and the one I picked (mainly for minimalism and readability) was
well documented enough that configuring a couple of supported features easily, such as configuring an "Archive" page.
I hadn't realised the first time around how key the "front matter" of Hugo content source files is, e.g. configuring
which "menu" a page will appear on.

I suppose (as usual) I should read the docs again, and understand more Hugo concepts. In the end it seems very flexible,
and has enough of a community around it that you can search for answers to questions like "[how to have posts with the
date in the URL](https://discourse.gohugo.io/t/dates-in-post-filenames/26219)".

But all in all to use a generator like Hugo effectively you still need to know your way around Git, the filesystem, and
think like a programmer, much like Github Pages.

So, now ready to write something, hopefully in less than 7 years. 😬️

