---
title: "Hybrid Mobile App development (2017)"
description: "Because I test all the technologies I can, as part of my job — and passion, people often ask me my thoughts on hybrid versus native mobile development. Let's see..."
image: /images/writing/state-mobile-cover.jpg
categories: blog
comments: https://github.com/jrmgx/public/issues/1
tags: [dev]
---

# Introduction

Because I test all the technologies I can, as part of my job — and passion,
people often ask me my thoughts on hybrid versus native mobile development.

I like to be radical in my life,
but on this subject I can't because people often already have a favorite
and will defend it against every rational arguments you can make.

What I try to remind people are that at the end, it is the usage that matters.

Native mobile versus hybrid is more than performances: it is about compromise.
I know and like native Android and iOS development, as well as web technologies,
so I consider myself not having a favorite (fake news alert: I have one, everybody does)

Let's dig in...

## Native development: No compromise

Native development is expensive!

To get an App on both stores — is there more than two stores? —
you will need two code bases, with two different languages (even if concepts are similar).

What you get in return is very fast Apps, well integrated in the mobile ecosystem
with a native access — fast and up to date — to all the device API.

## Webview based development: The (unresolved) promise

This one is an hybrid approach that use web technologies and thus is also often sold as cross-platform.

The idea with Webview based development, like Cordova,
is that you develop a specific version of your App with web technologies,
and get access to native API through a javascript bridge.

That way you have to develop only once and you can publish tour App to the store in no time! Yay!

That is the theory, and to be honest I have been convinced in the past.  
The truth is, web technologies were not made for native mobile development.

CSS for example is harder to get right than auto-layout (iOS),
and IDEs are not helping to get your views right, the code base quickly gets messy.
When finally you think you're done: nothing works as expected on other devices than the one you tested.
You discover the ugly truth: Webviews are distributed in different versions,
CSS does not work the same everywhere, even HTML sometime can get tricky.

Of course framework like Ionic can help a little.
It is a set of components here to help you implement common mobile pattern with all the CSS and performance tricks already included.
But when you test your App on a low-end device it is still slow as hell: in the end web is interpreted, nothing you can do here.

The end user will feel that the App has something wrong,
the responsiveness will be missing,
the look and feel won't match their expectations.

The time you think you would win from sharing the same code base
will be lost by trying to optimize the impossible,
by multiplying tricks for each platform and version.

## The best of both world? React-native

This one is promising.  
I tested it, and loved it, then I hated it, then I loved it, then I hated it, etc.

First, performances and look and feel are acceptable (if you stick to the official set of component)
Also to be honest I really like the ECMAScript 7 version of JavaScript... but I felt lost.

The framework has come with concept that are quite far from what I've been used to in mobile development.
I understand that people are not from the same background and that react-native try to solve some problem in its own way, but on Android and iOS these problems had already been tackled down, and they did use — mostly — the same patterns (that was the hate part).
Also react is quite far to standard 'old-fashioned' web development too, you need to learn the React way (if you're not used to it already).
The promise is sometime a bit daring: I thought I could use CSS, but not all of it is working, so I got quite frustrated.

When spending more time on it, I've got something working fast, and it was mostly the same code for both platforms, so in the end I was quite satisfied with that framework.

But today, I have framework fatigue.

The missing one: Xamarian

Ok, I'm ashamed but I never tested Xamarian, so I will only give you my sentiment.
I think it could be a good one, it has a full integrated IDE and compile everything to native code.
The C# language is pleasant to use too (used it a little already), this is something worth a test.

## Conclusion

Don't forgot the real question: what is your goal?

Today my personal recommendation is to go for a website only! (I get to be radical, happy me!)

Let's do a very good responsive, fast to load, PWA (progressive web app), even AMP compatible if needed,
using all you can get from the mobile platform. Push notification, home screen install, camera, location...

Then, if you got a good traction, you can start to think about mobile development.

Remember that the App stores are a non mercy place, no one download Apps any more (except games).
Plus the release cycle of Apps is a nightmare.  
I'm quite sure you don't need any of the fancy API these mobile platform offers — and that's ok.

So did you figured out what is my favorite?

Happy mobile development :)
