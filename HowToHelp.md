---
layout: defaults
title: Tidepool Project Contributions
published: true
---
#How you can help

##Use it

If you're a person with type 1 diabetes, please help test our apps and our platform. We'd love your feedback on user experience and functionality. Since our applications and platform are still under development, DO NOT USE OUR APPLICATIONS TO MAKE THERAPY ADJUSTMENTS.

If you'd like to learn more about about how you can participate in an IRB-approved pilot study, please sign up to our mailing lists for [blip](http://tidepool.org/blip) and [Nutshell](http://tidepool.org/Nutshell).

##Test it

###Manual QA

Sometimes, the best way to test a product is to bang on it. First, create an account to use blip. Track data, look at the results. Look for edge cases where data points are missing. Compare your data to the information displayed by blip. There should be nothing missing, nothing extra, and all the data you see should be accurate. 

If you find something wrong, tell us. If you find something confusing, tell us. If you don't find something you expected to find, tell us. We want all our software to be easy to use and to make information really clear. 

###Technical QA

Are you technically inclined with good attention to detail? Help with the testing of the software from a technical point of view.

Check out the repository and run the tests. Look at the test scripts, look for uncovered conditions, and write tests to check them. Submit pull requests. If you find bugs, write up the bugs and submit pull requests with code that validates the bug. If you want to fix the bug, a pull request that fixes the bug will also be appreciated!

##Doc it
###Documentation

It's pretty much a given that we don't have enough documentation. Can you help with writing it? The kinds of things we need help with include:

* Short, high-quality user documentation, with pictures, on how to get started with blip
* Help us write a set of FAQs
* Offer editorial fixes on anything we've published, from web pages to user docs to application docs.
* Work on improving this site.

To help with documentation, the easiest way for us is if you fork the [repository for this documentation site](https://github.com/tidepool-org/tidepool-org.github.io) and file a pull request. The site is using GitHub pages with Jekyll as the page generator. See README.md in the repository for how to get started; if you’re comfortable with git and a text editor, it’s pretty easy. If you're not comfortable with issuing pull requests, or just have a small edit, please [file an issue](https://github.com/tidepool-org/tidepool-org.github.io/issues). If you believe larger changes are needed and you're not sure how to do that through GitHub, please contact us and we'll try to find a mutually acceptable solution.

##Code it
###Code maintenance

We need help at all levels. 

_Things to know before you start:_

* Please read our architecture and design documents.
* Please read and follow our style guide.
* It's useful to read the issues list for the repositories you're interested in -- it may give you ideas for things to work on, or indicate work that's already underway.
* We work in Javascript and prefer to keep it that way. We're not interested at the moment in alternative implementations or languages. 
* We're willing to take small fixes from anyone. Big changes will need to come from people we know and trust. Best way to get to be one of those people is to make some small fixes first.
* In general, communications is key. You can work on things without telling us in advance, but a heads up may help us to help you help us.
* A pull request is the start of a discussion. Please don't get upset if we ask for additional changes after a review. We do that internally too.
* Small, tight fixes are easier to accept because they're easier to evaluate and a lot less likely to cause conflicts.
* When submitting a pull request, please explain your changes in the pull request body. A description of the changes made, the reason for them, and a reference to existing issue numbers is very helpful. Also, if there are other tasks or changes needed to complete the effort, please note that as well.

_Things you can do:_

* Pick an open bug and fix it; submit a pull request with the fix. If it's big, please check with us before you start so we know you're working on it and what your time frame will be. 
* Pick one of our repositories and study it. If you see ways to improve them, feel free to make the changes and submit a pull request, or to submit issues for the kinds of changes you'd like to see. Obviously, implementation is more useful than suggestion.
* Write unit tests. We can never have too many tests, as long as they're not redundant. Time spent exercising code we already have makes it easier to change that code. Help us push code coverage to 100% in each module!
* Small code improvements, code documentation, testability, making something more idiomatic, pulling out common code -- all of these are useful. Please submit pull requests, and keep them focused. 
* Features -- if you think we need a feature, a pull request is one way to propose it, especially for small features. But you're also encouraged and welcome to contact us about it before you do the work. Sometimes we have reasons why we don't or can't do things that might seem obvious to you. Or we may have conflicting work underway. A really good way to handle feature work is to create an issue on GitHub *before* you do the work, and let us know you intend to work on it. That way we can know it's coming and let you know if we see a conflict.
* Refactoring -- restructuring of code to make it more maintainable or testable is often really useful. If you decide to take on something like this, it would usually be wise to check with us beforehand. Refactorings can generate a lot of code conflict that can make them hard to accept. Also, when you refactor, please try not to add any features when you do. The same tests that ran before the refactoring should run after it as well.

#How to reach us

## Email

We intend to but have not yet set up mailing lists for development. When we do, we'll put the information here.

## IRC
Since our team spans the globe, there's nearly always someone in the #tidepool IRC channel on freenode.net. Feel free to pop in there and engage with us.
