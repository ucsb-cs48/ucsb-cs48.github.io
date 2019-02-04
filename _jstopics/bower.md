---
topic: Bower
desc: A general Package Manger for web app components
tags:
- package-managers
---


# What is Bower?

From [Bower's main web page](https://bower.io/) (retrieved 2016-06-19):

> Web sites are made of lots of things — frameworks, libraries, assets, and utilities. Bower manages all these things for you.
>
> Keeping track of all these packages and making sure they are up to date (or set to the specific versions you need) is tricky. Bower to the rescue!
>
> Bower can manage components that contain HTML, CSS, JavaScript, fonts or even image files. 
> Bower doesn’t concatenate or minify code or do anything else&mdash;it just installs
> the right versions of the packages you need and their dependencies.
>
> To get started, Bower works by fetching and installing packages from all over, 
> taking care of hunting, finding, downloading, and saving the stuff you’re looking for. 
> Bower keeps track of these packages in a manifest file, bower.json. 
> How you use packages is up to you. 
> Bower provides hooks to facilitate using packages in your tools and workflows.
>
> Bower is optimized for the front-end. If multiple packages depend on a package&mdash;jQuery for example&mdash;Bower
> will download jQuery just once. This is known as a flat dependency graph and it helps reduce page load.

# Using Bower with Jekyll and github pages

Various blog posts and tutorials (listed below) describes how to use bower with Jekyll site hosted on github pages. Note that these approach typically *require the developer to manually run Jekyll* to rebuild the site outside of github-pages; you can't just edit through the github GUI and expect github-pages to redeploy the site for you.     Instead, you have to manually regenerate the site after each change.   Depending on your workflow, this may be ok, or it may not be what you want to do.

* http://tscholak.github.io/update/guide/jekyll/2015/03/06/jekyll-and-github-pages.html
* http://www.aymerick.com/2014/07/22/jekyll-github-pages-bower-bootstrap.html
 
# References

* Bower main web page: <https://bower.io/>
