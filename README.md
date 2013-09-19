Developing R packages
================

As a modern statistician, one of the most fundamental contributions you can make is to create and distribute
software that implements the methods you develop. I have gone so far as to say if you write a paper without
software, [that paper doesn't exist](http://simplystatistics.org/2013/01/23/statisticians-and-computer-scientists-if-there-is-no-code-there-is-no-paper/).

The purposes of this guide are:

* To explain why writing software is a critical component of being a statistician.
* To give you an introduction into the process/timing of creating an R package.
* To help you figure out how to distribute/publicize your software.
* To remind you that ["the perfect is the enemy of the very good"](http://en.wikipedia.org/wiki/Perfect_is_the_enemy_of_good).
* To try to make sure Leek group software has a consistent design.<sup>1</sup>


Why develop an R package?
--------------------

Cause you know, you do what [your advisor](http://www.biostat.jhsph.edu/~jleek/) says and stuff.

But there are some real reasons to write software as a statistician that I think are critically important:

1. You probably got into statistics to have an impaact. If you work with Jeff it was probably to have an impact 
on human health or statistics. Either way, one of the most direct and quantifiable ways to have an impact on the
world is to write software that other scientists, educators, and statisticians use. If you write a stats method paper
with no software the chance of impacting the world is dramatically reduced. 
2. Software is the new publication. I couldn't name one paper written by a graduate student (other than mine) in the
last 2-3 years. But I could tell you about tons of software packages written by students/postdocs (both mine and at
other places) that I use. It is the number one way to get your name out there in the statistics community. 
3. If you write a software package you need for yourself, you will save yourself tons of time in sourcing scripts,
and remembering where all your code/functions are. 

Most importantly might be that creating an R package is _building something_. It is something you can point to and
say, "I made that". Leaving aside all the tangible benefits to your career, the profession, etc. it is maybe the
most gratifying feeling you get when working on research. 


When to start writing an R package
---------------------

As soon as you have 2 functions. Why 2? After you have more than one function it starts to get easy to lose track of
what your functions do, it starts to be tempting to name your functions _foo_ or _tempfunction_ or some other such 
nonsense. You are also tempted to put all of the functions in one file and just source it. That was what I did
with my first project, which ended up being an epically comical set of about 3,000 lines of code in one R file. 
Ask [my advisor](http://www.genomine.org/) about it sometime, he probably is still laughing about it. 


What you need
--------------------

To start writing an R package you need:

* R - I mean, unless you are a wizard.
* Your functions, each written in a separate file<sup>2</sup>.
* At minimum the following packages installed: _devtools, roxygen2, knitr_.
* If you are going to use C code: _Rcpp_
* A [github account](https://github.com/signup/free). 
* To read [Hadley's intro](http://adv-r.had.co.nz/Package-basics.html)

Naming your package
---------------------

The first step in creating your R package is to give it a name. Hadley [has some ideas](http://adv-r.had.co.nz/Package-basics.html)
about it. Here are our rules:

* Make it googleable - check by googling it.
* Make sure there is no Bioconductor/CRAN package with the same name. 
* Make it all lower case - people hate having to figure out caps in names of packages.
* Make it memorable and if you want serious people to use it - not too cute. 
* Make it as short as you possibly can while staying googleable.
* Never, under any circumstances, let Rafa or Hector name your package.<sup>3</sup> 


Versioning your package
---------------------

I like [Yihui's approach](http://yihui.name/en/2013/06/r-package-versioning/). Let's use that. 

Basically:

* Use format x.y.z.
* Increase z for minor bug fixes
* Increase y for minor changes
* Increase x for major changes

The only problem is we have to stay in line with Bioconductor's requirments because we want to publish most of our
packages to Bioconductor. For that reason:

* y must be odd when you submit a devel package to Bioc
* Let's make z 0 for packages we submit as devel to Bioc

Note that Bioc will bump the y value to an even value in release and make the new devel version the next number 
up from that. Once your package is on Bioc, you'll have to keep track of the two version numbers and make
sure that they are compatible. 


Creating your package
---------------------

1. Create a [new Github repository](https://help.github.com/articles/create-a-repo). 
2. Make the name of your repo the name of your package
3. Cre


Documentation
---------------------



Vignettes
---------------------


Simple >>>> Complex
---------------------



Who should be an author?
---------------------



[Good writers borrow from other authors, great authors steal outright](http://www.brainyquote.com/quotes/quotes/a/aaronsorki405048.html)<sup>4/sup>
---------------------


About the author
--------------------

The first version of this tutorial was written by Jeff Leek ([@simplystats](https://twitter.com/simplystats)). Hopefully
he can sucker his students into contributing since they are much, much better R programmers than he is. 

### Footnotes

1. These design requirements are subject to update and may not reflect Leek group software created before 9/18/2013 
(or ever really, remember the perfect is the enemy of the very good).
2. Except for utility functions, more on this later. 
3. Ask me about the time they named one of my packages "succs"
4. With proper attribution, of course.


