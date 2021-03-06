---
layout: post
title: Managing managed libraries with Scala and Eclipse
date: 2015-12-29
categories: articles
tags: [data science, Scala, sbt, Eclipse, functional programming]
comments: true
share: true
---

### My story [skip for the real answer]

I have become increasingly intrigued with the functional programming paradigm and its implementation in Scala.  I'm thoroughly enjoying working my way through Scala creator [Martin Odersky's coursera course] which I highly recommend to anyone from "timid and interested" to "hardcore."  As a data scientist with a statistics and economics background, I code every day in languages like R and Python wrangling data, implementing machine learning algorithms, tapping APIs and building data tools for data scientists.  However, I don't build [professional-grade] software.  Ive never taken a formal computer science course in college/grad school or learned Java.  Admittedly, I've felt overwhelmed at times by so many new concepts and tools with the course.  I'm simultaneously figuring out Scala, Java, sbt, eclipse, functional and object oriented paradigms.

As a learner poking my head in the field, I find it refreshing reading stories from fellow newcomers battling similar problems, even if they appear trivial in hindsight.  I also find it most effective teaching material when you still remember how you learned it.  Thus, this post and [likely, yet currently unwritten] subsequent Scala posts will be experimental learning notes rather than wisdom and tested best practices.

### Managed vs Unmanaged dependencies

One of the stumbling blocks I encountered on my first Scala project was a simple one: working with external libraries with sbt and Eclipse.  I started using unmanaged dependencies (downloading some jar files and pointing Eclipse to them) which worked well, until I encountered a library which had further dependencies -- I was working with [json4s].  More on [unmanaged vs managed dependencies here].  Managed dependencies have the advantage of being, well, managed.  You simply specify a library with a version and [Ivy] (a tool similar to Maven) will go out on the web and download it along with any dependencies.  

My issue was I initially couldn't get Eclipse to actually go out and download my dependencies.  As evidenced by [this StackOverflow post], there are probably many ways to achieve this.  The piece I was missing was using sbt in the Terminal while Eclipse was running to actually download the libraries.

### Make Eclipse recognize managed dependencies

Here's a step-by-step process

##### Step 1: Create a build.sbt file

Create **build.sbt** in the root project directory.  For example, my **build.sbt** looks like this:

{% highlight scala %}
name := "billscraper"

scalaVersion := "2.11.5"

libraryDependencies ++= Seq( 
  "org.json4s" %% "json4s-native" % "3.2.11",
  "org.json4s" %% "json4s" % "3.2.11",
  "io.spray" %% "spray-json" % "1.3.2",
  "org.scalanlp" %% "breeze" % "0.11.2"
)

{% endhighlight scala %}


##### Step 2: Open the Terminal

Assuming you're on mac, type the following in the Terminal:  

{% highlight bash %}
> sbt
> eclipse
{% endhighlight %}

You should see a bunch of jar files downloading... something like this:

<a data-flickr-embed="true"  href="https://farm2.staticflickr.com/1535/24025973066_8bcbcbdda5.jpg" title="downloading dependencies in sbt"><img src="https://farm2.staticflickr.com/1535/24025973066_8bcbcbdda5.jpg" width="467" height="318" alt="Screen Shot 2015-12-29 at 4.31.14 PM"></a>

##### Step 3: Refresh project in Eclipse

Now in Eclipse refresh your project by clicking F5, or right click on the project in the Package Explorer tab and click "Refresh"

##### Step 4: Check that it worked

Now to check that libraries have downloaded and Eclipse has recognized them.  

In Eclipse, right click on the project in the Package Explorer tab  
 &nbsp;&nbsp; => click "Build Path"  
 &nbsp;&nbsp; => click "Configure Build Path"  
 &nbsp;&nbsp; => click "Libraries" (icon with a stack of books)

Here you should see the libraries from build.sbt and all their dependencies.  Now you should be good to go -- import what you need from these libraries in your Scala code.  

<a data-flickr-embed="true"  href="https://farm2.staticflickr.com/1689/23944025972_bcf7686627_b.jpg" title="downloading libraries with sbt"><img src="https://farm2.staticflickr.com/1689/23944025972_bcf7686627_b.jpg" width="1024" height="498" alt="Screen Shot 2015-12-29 at 4.38.29 PM"></a>

### My environment

I configured my environment as recommended by the [Functional Programming Principles in Scala course]:

 * Mac - OSX Yosemite 10.10
 * Scala IDE build of Eclipse SDK 4.2.0
 * sbt 0.13.9


<!-- Links -->

[Martin Odersky's coursera course]: https://www.coursera.org/course/progfun 
[Functional Programming Principles in Scala]:  https://www.coursera.org/course/progfun 
[json4s]:https://github.com/json4s/json4s
[unmanaged vs managed dependencies here]: http://www.scala-sbt.org/0.13/tutorial/Library-Dependencies.html
[Ivy]:https://ant.apache.org/ivy/
[this StackOverflow post]: http://stackoverflow.com/questions/9070336/how-to-have-eclipse-recognize-dependencies-from-sbt
[Functional Programming Principles in Scala course]: https://www.coursera.org/course/progfun 
