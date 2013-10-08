Developing `R` packages
------------------

As a modern statistician, one of the most fundamental contributions you can make is to create and distribute
software that implements the methods you develop. I have gone so far as to say if you write a paper without
software, [that paper doesn't exist](http://simplystatistics.org/2013/01/23/statisticians-and-computer-scientists-if-there-is-no-code-there-is-no-paper/).

The purposes of this guide are:

* To explain why writing software is a critical component of being a statistician.
* To give you an introduction into the process/timing of creating an `R` package.
* To help you figure out how to distribute/publicize your software.
* To remind you that ["the perfect is the enemy of the very good"](http://en.wikipedia.org/wiki/Perfect_is_the_enemy_of_good).
* To try to make sure [Leek group](http://www.biostat.jhsph.edu/~jleek/) software has a consistent design.<sup>1</sup>


Data analyses
------------------

Stay tuned for the [Leek group](http://www.biostat.jhsph.edu/~jleek/) approach to building/sharing data analyses. 



Why develop an `R` package?
--------------------

Cause you know, you do what [your advisor](http://www.biostat.jhsph.edu/~jleek/) says and stuff.

But there are some real reasons to write software as a statistician that I think are critically important:

1. You probably got into statistics to have an impact. If you work with [Jeff](http://www.biostat.jhsph.edu/~jleek/) it was probably to have an impact 
on human health or statistics. Either way, one of the most direct and quantifiable ways to have an impact on the
world is to write software that other scientists, educators, and statisticians use. If you write a stats method paper
with no software the chance of impacting the world is dramatically reduced. 
2. Software is the new publication. I couldn't name one paper written by a graduate student (other than mine) in the
last 2-3 years. But I could tell you about tons of software packages written by students/postdocs (both mine and at
other places) that I use. It is the number one way to get your name out there in the statistics community. 
3. If you write a software package you need for yourself, you will save yourself tons of time in sourcing scripts,
and remembering where all your code/functions are. 

Most importantly might be that creating an `R` package is _building something_. It is something you can point to and
say, "I made that". Leaving aside all the tangible benefits to your career, the profession, etc. it is maybe the
most gratifying feeling you get when working on research. 


When to start writing an `R` package
---------------------

As soon as you have 2 functions. 

Why 2? After you have more than one function it starts to get easy to lose track of
what your functions do, it starts to be tempting to name your functions _foo_ or _tempfunction_ or some other such 
nonsense. You are also tempted to put all of the functions in one file and just source it. That was what I did
with my first project, which ended up being an epically comical set of about 3,000 lines of code in one `R` file. 
Ask [my advisor](http://www.genomine.org/) about it sometime, he probably is still laughing about it. 


What you need
--------------------

To start writing an `R` package you need:

* `R` - I mean, unless you are a wizard.
* Your functions, each written in a separate file<sup>2</sup>.
* At minimum the following packages installed: [devtools](http://cran.r-project.org/web/packages/devtools/index.html), [roxygen2](http://cran.r-project.org/web/packages/roxygen2/index.html) (suggested by [devtools](http://cran.r-project.org/web/packages/devtools/index.html)), [knitr](http://cran.r-project.org/web/packages/knitr/index.html).
* If you are going to use C code: [Rcpp](http://cran.r-project.org/web/packages/Rcpp/index.html)
* [Install git](https://help.github.com/articles/set-up-git).
* A [github account](https://github.com/signup/free). 
* To read [Hadley's intro](http://adv-r.had.co.nz/Package-basics.html).
* To read [Karl's Github tutorial](http://kbroman.github.io/github_tutorial/).

Naming your package
---------------------

The first step in creating your `R` package is to give it a name. Hadley [has some ideas](http://adv-r.had.co.nz/Package-basics.html)
about it. Here are our rules:

* Make it googleable - check by googling it.
* Make sure there is no [Bioconductor](http://www.bioconductor.org/)/[CRAN](http://cran.r-project.org/) package with the same name. 
* No underscores, dashes or any other special characters/numbers
* Make it all lower case - people hate having to figure out caps in names of packages.
* Make it memorable; if you want serious people to use it don't be too cute. 
* Make it as short as you possibly can while staying googleable.
* Never, under any circumstances, let [Rafa](http://rafalab.dfci.harvard.edu/) or [Hector](http://www.cbcb.umd.edu/~hcorrada/) name your package.<sup>3</sup> 


Versioning your package
---------------------

Almost all of our packages will eventually go on [Bioconductor](http://www.bioconductor.org/). So we are going to use the [versioning
scheme](http://www.bioconductor.org/developers/version-numbering/) that is compatible with that platform (with some helpful suggestions from [Kasper H.](http://www.biostat.jhsph.edu/~khansen/)).

The format of the version number will always be `x.y.z`. When you start any new package the version number should be
`0.1.0`. Every time you make any change public (e.g., push to [GitHub](https://github.com/)) you should increase `z` in the version number. If 
you are making local commits but not making them public to other people you don't need to increase `z`. You should 
stay in version `0.1.z` basically up until you are ready to submit to [Bioconductor](http://www.bioconductor.org/) (or [CRAN](http://cran.r-project.org/)) for release. 

Before release you can increase `y` if you perform a major redesign of how the functions are organized or are used. You
should never increase `x` before release. 

The first time you submit the package to [Bioconductor](http://www.bioconductor.org/) you should submit it as version number `0.99.z`. That way on the next 
release of [Bioconductor](http://www.bioconductor.org/) it will get bumped to `1.0.0`. The next devel version will get bumped to `1.1.0` on [Bioconductor](http://www.bioconductor.org/).
Immediately after releasing, if you plan to keep working on the project, you should bump your version on [GitHub](https://github.com/) to `1.1.0`.

Thereafter, again you should keep increasing `z` every time you make a public change. If you do a major reorganization
you should increase `y`. 


Creating your package
---------------------

Run this code from `R` to create your package. It will create a directory called _packagename_ and put some stuff in it 
(more on this stuff in a second). 

```S
## Setup
install.packages(c("devtools", "roxygen2", "knitr"))

## Load the libraries
library("devtools")
library("roxygen2")
library("knitr")

## Create the package directory
create("packagename")
```


Put your package on [GitHub](https://github.com/)
-----------------------

All packages that are developed by the [Leek group](http://www.biostat.jhsph.edu/~jleek/) will be hosted on [GitHub](https://github.com/). The accounts are free and
it makes it so much easier to share code/get other people to help you with your code. Here are the
steps to getting your package on [GitHub](https://github.com/):


1. Create a [new Github repository](https://help.github.com/articles/create-a-repo) with the same name (packagename)
2. In the _packagename_ directory on your local machine, run the commands: _git init_
3. Then run: _git remote add origin git@github.com:yourusername/packagename.git_
4. Create a file in the _packagename_ directory called README.md
5. Run the command: _git add *_
6. Run the command: _git commit -m 'initial commit'_
7. Run the command: _git push -u origin master_

In summary:

```bash
mkdir packagename
cd packagename
git init
git remote add origin git@github.com:yourusername/packagename.git
git add *
git commit -m 'initial commit'
git push -u origin master
```

* Use commit messages that will help you remember what you did and why you did it.
* If you interact very frequently with [GitHub](https://github.com) you will be interested on [setting up SSH keys](https://help.github.com/articles/generating-ssh-keys) to avoid typing your password every time you push/pull.
* You can mark specific versions of your package [using Git Tags](http://git-scm.com/book/en/Git-Basics-Tagging) which allows you to easily check the state of the package at that particular version.
* If more than one person is working on developing the package or you want to contribute to one, check [how to fork a repository](https://help.github.com/articles/fork-a-repo). It is an easy way to contribute with a very low burden on the maintainer and no setup.
* Consider whether you want users to report issues to your package. It is a very good framework for issue management, but can lead to duplicate information if the main issue reporting/tracking system is a mailing list like in the case of [Bioconductor](http://www.bioconductor.org/) packages. For an example of how GitHub's issue system looks like check the [rCharts issues](https://github.com/ramnathv/rCharts/issues?state=closed).

Once you're familiar with basic git and GitHub workflows, GitHub has some more advanced features that you can take advantage of. In particular, [github flow](http://scottchacon.com/2011/08/31/github-flow.html) is an excellent way to manage contributions, and [GitHub organizations](https://github.com/blog/674-introducing-organizations) can provide a central location for multiple people (e.g. in a lab) to collaborate on software.

The parts of an `R` package
--------------------

### `R` functions

The `R` functions you have written go in the __R/__ directory in the _packagename_ folder. Each of your `R` functions
should go in a separate file with a `.R` extension. We are going to use capital `R` for the extension of the files. 

Why? Don't ask questions. 

If you define a new class call the `.R` file _classname-class.R_. For example, if you are creating the leek class of objects
it would be called _leek-class.R_. If you are defining a new method for the class it should
be named _newclass-methodname-method.R_. For example, a plotting method for the leek class would go in a `.R` file
called _leek-plot-method.R_. 


### `DESCRIPTION `

The `DESCRIPTION` file is a plain text file that gets generated with the _devtools::create_ command. 

* The package name should go after the colon on the first line. 
* The package title should be a one sentence description of what the package actually does. 
* The description should be a one paragraph description that builds on the title. It should give a user
some idea about what kind of data your software should be used on, what the inputs are and what the outputs are. 
* The version should be defined as described above. 
* The authors field may have a @R before the colon which should be deleted. The authors should be in the format _author name <authoremail@host.com>_ for example: 
_Jeff Leek <jleek@jhsph.edu>_ and should be comma separated. 
* A maintainer field should be added with maintainers listed as comma separated files. You are the maintainer of your
package when you create it. See the section below on after you leave the [Leek group](http://www.biostat.jhsph.edu/~jleek/) for more information. 
* The dependencies (other `R` packages your software uses/depends on) should be listed in a comma separated list after
the `R` version. One of the dependencies should be the [knitr](http://cran.r-project.org/web/packages/knitr/index.html) package for the vignette. 
* The License is required to be open source. I like GPL-2 or GPL-3.
I like the creative commons licenses, like [CC-BY-SA](http://creativecommons.org/licenses/by-sa/2.0/), for
manuscripts, but [they are not recommended for software](http://wiki.creativecommons.org/Frequently_Asked_Questions#Can_I_apply_a_Creative_Commons_license_to_software.3F). 
[This](http://www.tldrlegal.com/) is a good website for learning more about software licenses. 
Also see [Jeff Atwood's comments on licenses](http://www.codinghorror.com/blog/2007/04/pick-a-license-any-license.html).
* You should add a line _VignetteBuilder: knitr_ 
* You should add a line _Suggests: knitr, BiocStyle_ 

For example:

```
Package: packagename
Type: Package
Title: A sentence
Version: 0.1.0
Date: 2013-09-30
Authors@R: c(person("Jeff", "Leek", role = c("aut", "cre", "ths"),
    email = "jleek@jhsph.edu"))
Depends:
    R(>= 3.0.2),
	knitr
Suggests:
    knitr,
	BiocStyle
Description: A couple sentences that expand the title
License: Artistic-2.0
```

Coding style requirements
---------------------

I will try to keep the stylistic requirements minimal because they would likely drive you nuts. For now there are:


1. Your indent should be 4 spaces on every line
2. Each line can be no more than 80 columns


You can set these as defaults (say in Sublime or Rstudio) then you don't have to worry about them anymore. If you find 
lines going longer than 80 columns, you will need to write the lines into fucntions, etc. 


Documentation
---------------------

This is how I feel about the relative importance of various components of statistical software development:

![documentation](https://raw.github.com/jtleek/rpackages/master/documentation.png)

Ideally your software is easy to understand and just works. But this isn't [Apple](http://www.apple.com/) and you don't have a legion
of test users to try it out for you. So more likely than not, at least the first several versions of your 
software will be at least a little hard to use. The first versions will also probably be slower than you would
like them to be. 

But if your software solves a real problem (it should!) and is well documented (it will be!) then people will
use it and you will have a positive impact on the world. 

Documentation has two main components. The first component is help files, which go into the __man/__ folder. The second 
component is vignettes which will go in a folder called __inst/doc/__ which you will have to create. I'll tackle 
each of these separately. 

### Help (man) files


These files document each of the functions/methods/classes you will expose to your users. The good news is that
you don't have to write these files yourself. You will use the [roxygen2](http://cran.r-project.org/web/packages/roxygen2/index.html) package to create the man files. To use
[roxygen2](http://cran.r-project.org/web/packages/roxygen2/index.html) you will need to document your functions in the `.R` files with comments formatted in a specific way. Right
before your functions you should have a set of comments that are denoted by the symbol _#'_. They are structured in 
the following way:

```S
#' A one sentence description of what your function does
#' 
#' A more detailed description of what the function is and how
#' it works. It may be a paragraph that should not be separated
#' by any spaces. 
#'
#' @param inputParameter1 A description of the input parameter \code{inputParameter1}
#' @param inputParameter2 A description of the input parameter \code{inputParameter2}
#'
#' @return output A description of the object the function outputs 
#'
#' @keywords keywords
#'
#' @export
#' 
#' @examples
#' R code here showing how your function works

myfunction <- function(inputParameter1, inputParameter2){
	## Awesome code!
	return(result)
}
```
You include the _@export_ command if you want the function to be exported (i.e. visible) to your end users. [Hadley](http://had.co.nz/) has a pretty comprehensive [guide](http://adv-r.had.co.nz/Documenting-functions.html) where you can 
learn a lot more about how _roxygen_ works. Your function follows immediately after the comments. 

When you have saved functions with [roxygen2](http://cran.r-project.org/web/packages/roxygen2/index.html) style comments you can create the `.Rd` files (the man files themselves) 
by running:

```S
library("devtools")
document("packagename")
```
on the package folder. The package folder must be in the current working directory where you are editing. 

Please read [Hadley](http://had.co.nz/)'s [guide](http://adv-r.had.co.nz/Documenting-functions.html) in its entirety to understand how to document packages and in particular, how [roxygen2](http://cran.r-project.org/web/packages/roxygen2/index.html) 
deals with [collation](http://cran.r-project.org/doc/manuals/R-exts.html#The-DESCRIPTION-file) and [namespaces](http://cran.r-project.org/doc/manuals/R-exts.html#Package-namespaces). 


### Vignettes

Documentation in the help files is important and is the primary way that people will figure out your functions if they
get stuck. But it is equally (maybe more) critical that you help people _get started_. The way that you do that is to
create a vignette. For our purposes, a vignette is a tutorial that includes the following components:

* A short introduction that explains 
  * The type of data the package can be used on
  * The general purpose of the functions in the package
* One or more example analyses with
  * A small, real data set
  * An explanation of the key functions
  * An application of these functions to the data
  * A description of the output and how it can be used
  
We will write Vignettes in [knitr](http://yihui.name/knitr/). We will put a file called `vignette.Rmd` in
the directory __packagename/inst/doc/__. [Here](http://yihui.name/knitr/demo/vignette/) is some information
from [Yihui](http://yihui.name/) about building vignettes in [knitr](http://cran.r-project.org/web/packages/knitr/index.html). You should use the [BiocStyle](http://www.bioconductor.org/packages/devel/bioc/html/BiocStyle.html)
package to style your vignette. This means you will need to add this code to the preamble of your markdown
file:

```S
<<style, eval=TRUE, echo=FALSE, results='asis'>>=
BiocStyle::latex()
@
```

See the [BiocStyle vignette](http://www.bioconductor.org/packages/devel/bioc/vignettes/BiocStyle/inst/doc/LatexStyle.pdf) for
commands that you can use when creating your vignette (e.g. _\Biocpkg{IRanges}_ for referencing a [Bioconductor](http://www.bioconductor.org/) package).



Who should be an author?
---------------------

For our purposes anyone who wrote a function exposed to users in the package (a function that has an @export in the documentation)
will be listed as an author. 

Who should be a maintainer
---------------------

__You__ will be a maintainer and [Jeff](http://www.biostat.jhsph.edu/~jleek/) will be a maintainer. If you can sucker one of your fellow students into maintaining the package
as well, you can list them, but they must make the same commitment to 5 years of support. 

[Good writers borrow from other authors, great authors steal outright](http://www.brainyquote.com/quotes/quotes/a/aaronsorki405048.html)<sup>4</sup>
---------------------

One thing I think a lot of academics (definitely including myself here) struggle with is the need to be "new" and 
"innovative" with everything they do. There is a strong [selective pressure](http://en.wikipedia.org/wiki/Evolutionary_pressure)
for these qualities in academia.

But when writing software it is very, very important not to reinvent every single wheel you see. One person who 
is awesome at blending existing tools and exponentially building value is [Ramnath Vaidyanathan](https://github.com/ramnathv).
He built [slidify](https://github.com/ramnathv/slidify) on top of [knitr](http://cran.r-project.org/web/packages/knitr/index.html) and [rCharts](https://github.com/ramnathv/rCharts) 
on top of existing [D3](http://d3js.org/) libraries. They allowed him to create awesome software without having to solve every single problem.

Before writing general purpose functions (say for regression or for calculating p-values or for making plots) make sure
you search for functions that already exist (or ask [Jeff](http://www.biostat.jhsph.edu/~jleek/)/your fellow students if you aren't sure).

An important and balancing principle is that you should try to keep the number of dependencies for your package as 
small as possible. You should also try to use packages that you trust will be maintained. Some ways to tell if a package
is "trustworthy" are to check the number of downloads/users (higher is better), check to see if the package is being
actively updated (on [GitHub](https://github.com/)/[Bioconductor](http://www.bioconductor.org/)/[CRAN](http://cran.r-project.org/)) and there is a history of updates, and check to see if the authors of the
packages routinely maintain important packages (like [Hadley](http://had.co.nz/), [Yihui](http://yihui.name/), [Ramnath](https://github.com/ramnathv), [Martin Morgan](http://www.fhcrc.org/en/util/directory.html?q=martin+morgan&short=true#peopleresults), etc.). Keep in mind your
commitment (see below) when making decisions about whose functions to use. 

Simple >>>> Complex
---------------------

A major temptation for everyone creating code is to generate a bunch of features they think that users will want without
actually testing those features out. The problem is that each new feature you create in your package will monotonically
increase the number of dependencies and the amount of code you have to maintain. In general, the principle should be
to create exactly enough functions that the users can install your package, perform your analysis, and return the results,
__with no extraneous functionality__.

Specifically, be wary of things like GUIs or [Shiny](http://www.rstudio.com/shiny/) apps. Given the heavy emphasis placed on reproducibility these days,
it is rarely the case that real/important analyses will be performed in a point and click format. 

If you are way into creating products that point-and-click users will be interested in I'm very happy to support you in 
that, since I think those things are cool, probably the future, and can certainly raise the interest in your work. But
they present a potentially major difficulty in maintainence and should be placed in separate packages on your own account. 


Unit tests
---------------------

Unit tests are an important for the following reasons:

* They make it easier for you to find bugs in your code
* They make it easier for you to figure out if your code works together
* __They make you slow down and think about what you are doing__


Your advisor tends to rush through things. While there is a strong interest in being competitive/getting things done in the 
[Leek group](http://www.biostat.jhsph.edu/~jleek/), there is an even stronger interest in doing them right and for the long term. 
We will use the [testthat](http://adv-r.had.co.nz/Testing.html) framework for building unit tests. A couple of suggestions
for using the framework are:

* Organize your tests into contexts (groups of tests) and name them.
* Have some tests that test the basic helper functions on fixed data.
* Have some tests that check the output of your main functions on fixed data. 
* Have some tests that can be run on random data - these can help find bugs before they happen


To use the [testthat](http://adv-r.had.co.nz/Testing.html) package you will put a file called _test-area-packagename.R_ in
the `inst/tests` directory, where _area_ is the name of the broad class of functions you are testing. Then add
another file called _run-all.R_ in the same directory. The _run-all.R_ function has this code in it:


```S
library(testthat)
library(mypackage)
test_package("mypackage")
```

This means that whenever you run `R CMD check` on your package, you will get an error if one of the unit tests fails. This
is important, because it will be one way for me to check your code and to tell you if you have broken your code
with an update. 


### An example unit test

Here is an example unit test, so you get the idea of what I'm looking for. Below is a function for performing
differential expression analysis on a matrix. 


```S
#' A function for differential expression analysis
#' 
#' This function takes a matrix of gene expression data
#' (genes in rows, samples in columns) and a factor variable
#' with two levels and performs differential expression analysis
#'
#' @param data A gene expression data matrix with genes in rows and samples in columns
#' @param grp A two-level factor variable with two levels
#'
#' @return pValues The p-values from the differential expression test.
#'
#' @keywords differential expression
#'
#' @export
#' 
#' @examples
#' R code here showing how your function works

deFunction <- function(dat, grp){
  if(class(grp)!="factor"){stop("grp variable must be a factor")}
  if(length(unique(grp))!=2){stop("grp variable must have exactly two levels")} 
  if(any(genefilter::rowSds(dat)==0)){stop("some genes have zero variance")} 
  result = genefilter::rowttests(dat,grp)$p.value
  return(result)
}

```

Now create a new file called _test-defunction.R_. The code in that file is: 


```S
context("tests on inputs")

test_that("tests for grp variable",{
  set.seed(12345)
  dat <- matrix(rnorm(100*30),nrow=100,ncol=30)
 
  grp <- rep(c(0,1),each=15)
  expect_that(deFunction(dat,grp),throws_error("grp variable must be a factor"))
  
  grp <- as.factor(rep(c(0,1,2),each=10))
  expect_that(deFunction(dat,grp),throws_error("grp variable must have exactly two levels"))
  
  grp <- as.factor(rep(0,30))
  expect_that(deFunction(dat,grp),throws_error("grp variable must have exactly two levels")) 
})

test_that("tests for dat variable",{
  set.seed(12345)
  grp <- as.factor(rep(c(0,1),each=15))
  
  dat <- matrix(0,nrow=100,ncol=30)
  expect_that(deFunction(dat,grp),throws_error("some genes have zero variance; t-test won't work"))
})

context("test on outputs")

test_that("test p-values are numeric and non-zero",{
  set.seed(12345)
  grp <- as.factor(rep(c(0,1),each=15))
  dat <- matrix(matrix(rnorm(100*30)),nrow=100,ncol=30)
  
  expect_that(deFunction(dat,grp),is_a("numeric"))
  expect_that(all(deFunction(dat,grp) > 0),is_true())
})

```

If you run the command: `test_file("test-defunction.R")` you should get the output:

```S
tests on inputs : ....
test on outputs : ..
```

without any error messages. But if you accidentally delete one of your error checking messages at the beginning of
the function and rerun the tests, it will tell you which context and which test broke. 

The thing to keep in mind here is that you would be doing these tests on the fly/manually anyway. So you might as well
write them into a set of unit tests that will give you an idea of where your functions are breaking. Some things that 
you should be testing:

* That your error messages catch what they should
* That your outputs are what you expect them to be
* That you get reasonable answers for reasonable inputs
* When you know what the answer should be - you get it





Dummy proofing
---------------------

Hopefully your package will have a ton of users. Inevitably, they will try to use the software for purposes you did not
intend. Some of these will be happy things (software you wrote for microarrays being used in sequencing). Sometimes
they will be unhappy - people using it completely out of context and getting wonky answers. 

A major component of dummy proofing is writing thorough documentation and vignettes (see the above sections). 
When you see weird cases (reported from users or cases you find yourself) add them to the documents. There is one
additional important component of dummy proofing we will use: input dummy proofing (related to unit testing). 

### Input dummy-proofing

Most of the time, if you input non-matching arguments or arguments of the wrong class into R functions a cryptic
error will result. This will cause you headaches when maintaining your software. So you should add some commands
with the `stop` function at the beginning of each function that throw errors if the arguments don't have the correct
class or would result in silly output (like the zero variance gene test in the unit testing example above). 



Releasing to [Bioconductor](http://www.bioconductor.org/)
---------------------

Once you have developed a package you should use it yourself for a couple of weeks. Then you should 
have at least one other student use it without you giving them any instructions other than telling them
where some data are - that way you can test your documentation. Finally, you should meet with [Jeff](http://www.biostat.jhsph.edu/~jleek/) and 
get ready to release it to [Bioconductor](http://www.bioconductor.org/). 

The steps in releasing to [Bioconductor](http://www.bioconductor.org/) are:

* Follow [Bioconductor](http://www.bioconductor.org/)'s [checklist](http://www.bioconductor.org/developers/package-submission/) to confirm
the package is ready to upload.
	* In particular, check that your package passes R CMD check in less than 5 minutes. You can use
	
```S
library("devtools")
check_doc("packagename") ## Only for checking the documentation
system.time(check("packagename")) ## R CMD check with time information
```

* Update the version number and push to [GitHub](https://github.com/). In the commit comments, state it is the version being 
pushed to [Bioconductor](http://www.bioconductor.org/).
* Send an email as described in the checklist stating that you want an account and want to submit a package. 
* Submit the package to [Bioconductor](http://www.bioconductor.org/).
* Update the version number (bump `y` and `z`) to the next odd number for `z`. In the commit comments, state
that this is the new devel version. 


Your commitment
---------------------

You are vested in creating the software you are working on now since it is part of your training and will definitely
help your short term career in terms of getting jobs/gaining visibility. But your time as a graduate student is
(happily for you) limited. After a few years you will graduate and head off to an awesome job. The software you 
built as a student may be less important to you. 

Hopefully, though, it is still very important to the users who are applying that software to solve the most pressing
problems in molecular biology and medicine. It is unfair to those users if your software breaks down and can't be
installed/used. 

So if you undertake a research project in the [Leek group](http://www.biostat.jhsph.edu/~jleek/) which involves software development (pretty much all of them will)
you commit to maintaining that software for at least 5 years after you graduate. Of course, I can't hold you to that
like a contract or anything, but think of it as an honor thing. 

5 years is a long time. It is most of the way toward tenure (in academia) or probably 3 years after you have started your
own awesome company and appeared on [Techcrunch](http://techcrunch.com/). So it is worth thinking about ways you can ensure that the maintainence
will be as low as possible. Specifically:

* Make the dependencies as minimal as possible. If your dependencies update, you'll have to update the software.
* Only create functions that are absolutely necessary for the package. It is hard to delete functions from a package
after it is released without messing with users and adds to the maintainence headache each time you keep one in. 
* Make the vignette really clear and add a FAQ with questions you get from users while you are still in the [Leek group](http://www.biostat.jhsph.edu/~jleek/). 
* Add comments to your code/unit tests so that when something breaks you can find/fix the problem quickly. 


About the author
--------------------

The first version of this tutorial was written by [Jeff Leek](http://www.biostat.jhsph.edu/~jleek/)  ([@simplystats](https://twitter.com/simplystats)). Hopefully
he can sucker his students into contributing since they are much, much better `R` programmers than he is.

[Titus Brown](http://ivory.idyll.org/blog) added the GitHub flow and organization links.  You can see his group's setup at [ged-lab](http://github.com/ged-lab).

### Footnotes

1. These design requirements are subject to update and may not reflect [Leek group](http://www.biostat.jhsph.edu/~jleek/) software created before 9/18/2013 
(or ever really, remember the perfect is the enemy of the very good).
2. Except for utility functions, more on this later. 
3. Ask me about the time they named one of my packages "succs".
4. With proper attribution, of course.


