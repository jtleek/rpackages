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

1. You probably got into statistics to have an impact. If you work with Jeff it was probably to have an impact 
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

As soon as you have 2 functions. 

Why 2? After you have more than one function it starts to get easy to lose track of
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
* [Install git](https://help.github.com/articles/set-up-git)
* A [github account](https://github.com/signup/free). 
* To read [Hadley's intro](http://adv-r.had.co.nz/Package-basics.html)
* To read [Karl's Github tutorial](http://kbroman.github.io/github_tutorial/)

Naming your package
---------------------

The first step in creating your R package is to give it a name. Hadley [has some ideas](http://adv-r.had.co.nz/Package-basics.html)
about it. Here are our rules:

* Make it googleable - check by googling it.
* Make sure there is no Bioconductor/CRAN package with the same name. 
* No underscores, dashes or any other special characters/numbers if possible
* Make it all lower case - people hate having to figure out caps in names of packages.
* Make it memorable and if you want serious people to use it - not too cute. 
* Make it as short as you possibly can while staying googleable.
* Never, under any circumstances, let Rafa or Hector name your package.<sup>3</sup> 


Versioning your package
---------------------

Almost all of our packages will eventually go on Bioconductor. So we are going to use the [versioning
scheme](http://www.bioconductor.org/developers/version-numbering/) that is compatible with that platform (with some helpful suggestions from [Kasper H.](http://www.biostat.jhsph.edu/~khansen/)).

The format of the version number will always be x.y.z. When you start any new package the version number should be
0.1.0. Every time you make any change public (e.g., push to Github) you should increase z in the version number. If 
you are making local commits but not making them public to other people you don't need to increase z. You should 
stay in version 0.1.z basically up until you are ready to submit to Bioconductor (or CRAN) for release. 

Before release you can increase y if you perform a major redesign of how the functions are organized or are used. You
should never increase x before release. 

The first time you submit the package to Bioconductor you should submit it as version number 0.99.z. That way on the next 
release of Bioconductor it will get bumped to 1.0.0. The next devel version will get bumped to 1.1.0 on Bioconductor.
Immediately after releasing, if you plan to keep working on the project, you should bump your version on Github to 1.1.0.

Thereafter, again you should keep increasing z every time you make a public change. If you do a major reorganization
you should increase y. 

Creating your package
---------------------

Run this code from R to create your package. It will create a directory called _packagename_ and put some stuff in it 
(more on this stuff in a second). 

```S
## Load the libraries
library(devtools)
library(roxygen2)
library(knitr)

## Create the package directory
create("packagename")
```


Put your package on Github
-----------------------

All packages that are developed by the Leek group will be hosted on Github. The accounts are free and
it makes it so much easier to share code/get other people to help you with your code. Here are the
steps to getting your package on Github:


1. Create a [new Github repository](https://help.github.com/articles/create-a-repo) with the same name (packagename)
2. In the _packagename_ directory on your local machine, run the commands: _git init_
3. Then run: _git remote add origin git@github.com:yourusername/packagename.git_
4. Create a file in the _packagename_ directory called README.md
5. Run the command: _git add ._
6. Run the command: _git commit -m 'initial commit'_
7. Run the command: _git push -u origin master_



The parts of an R package
--------------------

### R functions

The R functions you have written go in the R/ directory in the _packagename_ folder. Each of your R functions
should go in a separate file with a .R extension. We are going to use capital R for the extension of the files. 

Why? Don't ask questions. 

If you define a new class call the .R file _classname-class.R_. For example, if you are creating the leek class of objects
it would be called _leek-class.R_. If you are defining a new method for the class it should
be named _newclass-methodname-method.R_. For example, a plotting method for the leek class would go in a .R file
called _leek-plot-method.R_. 


### DESCRIPTION 

The DESCRIPTION file is a plain text file that gets generated with the _create_ command. The packagename should
go after the coon on the first line. The package title should be a one sentence description of what the package
actually does. The version should be defined as described above. The authors field may have a @R before the colon
which should be deleted. The authors should be in the format _author name <authoremail@host.com>_ for example: 
_Jeff Leek <jleek@jhsph.edu>_ and should be comma separated. 



Documentation
---------------------

This is how I feel about the relative importance of various components of statistical software development:

![documentation](https://raw.github.com/jtleek/rpackages/master/documentation.png)

The most frustrating thing 


Vignettes
---------------------


Simple >>>> Complex
---------------------



Who should be an author?
---------------------



[Good writers borrow from other authors, great authors steal outright](http://www.brainyquote.com/quotes/quotes/a/aaronsorki405048.html)<sup>4</sup>
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


