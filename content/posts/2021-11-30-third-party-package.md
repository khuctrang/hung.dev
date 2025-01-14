---
template: post
title: How to choose a third party package
slug: choose-third-party-package
draft: false
date: 2021-11-29T17:24:56.135Z
description: Guideline to choose a third party package
category: "Blog"
tags:
  - npm
  - third party package
  - library
  - javascript
  - programming
socialImage: "/media/npm-start.jpg"
---
![Reinvent the wheel](/media/wheel.jpg)

_I would like to express my sincere thanks to [@tamhoang1412](https://www.linkedin.com/in/tamhoang1412/) and [@mattmurph9](https://github.com/mattmurph9) for reviewing and proofreading this post._
## Introduction

A wise man once said "don't reinvent the wheel". It's true for every industry, including software development. When developing a feature, sometimes you are in a situation "I need a third party package". So, how to correctly choose a library or package? In the past, whenever I thought I needed an external library, I just googled it (e.g: react charting libraries), chose a random package, then tried it. If it worked, I used it. But it turned out that this strategy was not optimal. In this article, I will share some personal tips for choosing a javascript package. But these tips can be applied to any other language, framework, or even when choosing a language for your team.

### 1. Fit your need
![npm start](/media/npm-start.jpg)

It sounds obvious that you should choose a solution that solves your problem. A package usually tries to solve a set of problems in general. Meanwhile, your use case is particular to your situation, and the package might not fit. Please make sure the third party code you are going to install handles sufficiently, but not too much. Don't over-engineer a solution. Many times, [DIY](#7-diy) is a good option.

### 2. Easy to adopt
![Easy](/media/easy.jpg)

In the industry, you don't work alone. So, pick a solution that your team members are able to adapt to quickly. You don't want to choose a solution that only you can implement and maintain.

### 3. Popularity
![Popularity](/media/popular.jpg)

It's usually true that a good solution is a popular one (if a solution is good, why isn't it popular)? I measure the popularity of a javascript package by the number of github stars and the number of weekly downloads on [https://npmjs.com](https://npmjs.com). A package with a large community proves that it solves the problem for many people. It comes with the advantage of having better support when we run into an issue. A more popular package will have more maintainers, contributors and users, so it's more mature than a less popular one. It usually has good documentation and tutorials, which help to easily adopt and use it correctly and efficiently.

**Bonus:** [https://stateofjs.com/](https://stateofjs.com/) is a great resource for you to know about what's trending in javascript each year

### 4. In active development
![Team work](/media/teamwork.jpg)

It's very important that you are choosing an active project instead of a dead/unmaintained project. An active project improves over time through community feedback. An unmaintained project does not move forward, fix functional bugs or patch security issues. Sometimes, a very popular package can be abandoned and go into a "frozen" state with many open issues and pull requests. It might have been a great solution in the past, but this is a sign that we have to move on. An example is [react-loadable](https://github.com/jamiebuilds/react-loadable). It was a great solution for a very long time for code-splitting in React. I totally loved it. But it's stale now with many issues and PRs since 2018 (this post is written at the end of 2021). Now, if I need to split code in React, I use [loadable-components](https://github.com/gregberge/loadable-components), which is in active development, becoming more popular, patches bugs reported by the community, and most importantly, solves my problems. My personal advice: choose a package that's active in the last 3-6 months, with issues that are being resolved and PRs that are being merged.

### 5. License
![License](/media/license.jpg)

It's very important that the package you install grants you the permission to use it for commercial purpose in production. Packages in the wild are great. But not all of them are free to use. 

Let's take an example. If you are working with charts, you might have heard of [Highcharts](https://github.com/highcharts/highcharts) - a rock-solid and incredibly flexible charting library made for developers. You download it from [npm](https://www.npmjs.com/), make a chart, it looks great, and you are ready to roll out the feature. But please note that Highcharts is not free for commercial use. So if you are writing code for your company and your company hasn't bought a license to use this package in production, you are basically breaking the law. We don't want that, right? So please buy a license before using Highcharts in production. Myself, I use [Recharts](https://github.com/recharts/recharts) instead, a free package powered by [D3](https://github.com/d3/d3) with MIT license and in very active development.

Software licenses are quite complicated and go beyond the purpose of this post. But in general, packages with MIT, Apache 2.0 and BSD are safe to use for commercial purpose. If you choose a package with a license that is not one of those, please read the license carefully.

### 6. Reasonable size
![Size](/media/size.jpg)

You don't want to bloat your web application's bundle size by installing a 700kb package. Sometimes it is necessary, but most of the time it is not. Please consider the size of a library before making a decision. You need to know about the unpacked size, the bundle size, and the gzipped size. A larger package means you need more hard disk and bandwidth, which means more money you need to pay, also it takes a longer time for users to load your app.

### 7. DIY
![Do it yourself](/media/diy.jpg)

As mentioned in [Fit your need](#1-fit-your-need), many packages try to solve a general problem (thus the size of the package is large). You may only need a small part of the package. Sometimes, your problem is unique and there are no existing third party packages out there that solve it. In those cases, it's a great time for you to do it yourself. I found myself in the early days in the industry spending much time finding a third party package to help me build features. But over time, I more rarely used external packages for my daily tasks. It doesn't mean that I always reinvent the wheel. It means that I know what I am doing and I can seek help from the community when I truly need to (for example I will never sanitize user input by myself, but use [DOMPurify](https://github.com/cure53/DOMPurify))

**Bonus:** For some problems, if you cannot find a package that helps you, make one and give it to the world by making it an open source software. The open source sofware community will give you back many things, more than you can expect.

## Conclusion

Above are my very personal tips, they might be true and they might not be true. If there's something you don't agree with, that's OK. Just ignore it, or better, let me know why you don't agree. If you have additional tips, please let me know in the comment section. I would love to hear from you. I hope this helps you choose the right packages for you and your team. Happy coding!