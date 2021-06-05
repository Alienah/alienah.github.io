---
layout: post
title:  "Blessed Build Tools & Task Runners"
subtitle : "Grunt, Gulp, Webpack."
date:   2018-02-11 12:00:00 -0400
header-img: "img/posts/blessed-task-runners_background.png"
author: "Aida Albarrán and Mónica Aguado"
comments: true
tags: [TipsAndResources, Webpack]
---

Many of the tasks we have to do as developers are repetitive such as refreshing the browser when making a change, compressing the js and css file. In addition, we have to minimize our code to gain performance.

Task runners allows us to automate all those tasks in order to take advantage of the time in more productive activities. Time is the most valuable asset, and the more tasks we can automate, the more time we can focus on important things.

Gulp vs Grunt is the most popular dilemma. They both automate tasks, use Node.js, and they are both powerful and efficient.

On the other hand, Webpack is a bundler and it comes in handy when you need to pack multiple assets (images, fonts, etc.) together into a dependency graph. Webpack is often used in combination with Gulp.

Well, are you starting to see something useful in these tools? We are sure that if you tried them, you would fall in love. Let’s see each one with a little more detail.

## What is Grunt?

![Grunt logo](/img/posts/blessed-task-runners_grunt.png)

As we have said before, grunt is a “task runner”, that is, a tool to automate repetitive tasks such as css compilation, test execution etc. In a JS file called Gruntfile.js we write the tasks that we want to automate and with we execute a simple command.

To install grunt, it is best to do it globally with ```npm install -g grunt-cli``` and then in our project we define the tasks in the grunt file. The best thing about this tool is that we already have many plugins and tasks created [here](http://gruntjs.com/plugins)

Did you know that there are major companies using it? Some examples are the following (see image).

![Companies using grunt, as Twitter or Adobe](/img/posts/blessed-task-runners_grunt-companies.png)


## What is Gulp?

![Gulp logo](/img/posts/blessed-task-runners_gulp.jpeg)

Gulp is an automated tasks tool and you have to run it with the console or command terminal. This means that Gulp doesn’t have a GUI and you have to config and install it without help. But, thanks to that, you can configure it as you wish and process tasks to improve performance, plus you have the possibility to adapt it to when you need it.

Npm is our friend, because it helps us to install Gulp. You have to write in the console:

``` npm -g install gulp-cli``` 

Then you need a new file called *package.json*, that it’s a JSON file. When finish installing, you write gulp in the command terminal and a new window will open to see your website. You can find more information about the installation here.

The best things in this task runner are:

* Flow control and more clarity of processes
* More than 2770 plugins for different purposes among which each one is for a particular task
* You need less code than Grunt
* Faster because Gulp have a better stream usage and in-memory operations

Are you ready to use it?

Some of the sponsors are the next ones:

![Companies sponsors of gulp](/img/posts/blessed-task-runners_gulp-companies.png)

## What is Webpack?

![Webpack logo](/img/posts/blessed-task-runners_webpack.png)

In the webpack web this is defined as a module bundler and basically what it does is help us with fragmentation. All the pages of our site do not need the same JS code, for example if we have a map in a section with enough functionality JS, we do not have to drag that code to the entire web. In this, webpack helps us.

To install it we just have to run:

```npm install -g webpack.```

Here is a post with more information:

<https://medium.com/@dabit3/beginner-s-guide-to-webpack-b1f1a3638460#.lznacz56t>

There are many organizations sponsoring webpack as for example:

![Companies using webpack, as Trivago, Slack or Airbnb](/img/posts/blessed-task-runners_webpack-companies.png)
![Companies using webpack, as Trivago, Slack or Airbnb](/img/posts/blessed-task-runners_webpack-companies2.png)
![Companies using webpack, as Trivago, Slack or Airbnb](/img/posts/blessed-task-runners_webpack-companies3.png)



As you can see, more and more developers are joining the use of these wonderful tools that save us a lot of time, which is better to invest in something more profitable. We hope you found our proposal interesting and that you get in on the automation cart!

Autors: Aida Albarrán and [Mónica Aguado](https://medium.com/@MoniAguado)

Image of background source: blog.qmo.io