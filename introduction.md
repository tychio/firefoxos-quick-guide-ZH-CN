# 简介 {#introduction}

## Firefox OS

![Firefox OS](images/originals/firefox_os_simulator.png)

[Firefox OS](http://www.mozilla.org/firefox/os/)是一个由[Mozilla](http://mozilla.org)及其合作伙伴开发的新的移动平台。许多国家的移动设备都可以运行Firefox OS，并且在今年这个范围还将拓展。针对发展中市场，Firefox OS 的使命是带来下一个十亿在线人数。为了达成它，Firefox OS设备被构建成了一个类似在价格上具有竞争力的伟大第一智能手机。Firefox OS设备与高端智能手机是不可比的，比如苹果iPhone 5S和三星Galaxy S4。它们被构建成功能手机的替代品，当人们使用上述设备时，可以用负担得起的花费升级为Firefox OS并获得所有智能手机体验。

例如在巴西和哥伦比亚这样的发展中市场，对一般消费者来说一部性能优越的智能手机一般是非常昂贵的。人们可以购买廉价手机，但被那些手机所使用的平台是为高端设备准备的。因此，那些硬件表现不佳的手机会导致一个很糟的用户体验。Firefox OS 是专门为在有限的硬件上运行并提供一个相当不错的用户体验而设计的。

另一个Firefox OS区别因素是它的开放性。考虑到在目前主流移动操作系统中每个厂商都有特权去推动其自有方式，而不顾开发者和用户想要的，这些操作系统简直是私有粮仓（还记得苹果尝试在iTunes应用商店中禁止除了Objective-C以外的其他语言吗？）。在这些私有的生态系统中你只能在授权的平台上发布你的应用 - 而且厂商通常会很大比例的占具从其设备上产生的任何购买中的获利。

除了强迫开发者到私有发布平台，这些系统还强迫你使用他们的软件开发工具（SKD）。如果你想要使用官方工具去构建一个iOS和Android的原生应用，你将会需要使用Objective-C和Java去编码一个应用。在编码层面上，这意味着一个开发者在项目间重用的代码将非常少（也许重用一些媒体资源）。那种工作需要开发者学习两种语言构建同样的软件两次。

Firefox OS有别于使用“HTML5”作为开发平台的竞争对手。HTML5是一个市场术语，常常指不断发展的Web标准的集合，比如HTML，CSS和Javascript。那些免费版税的标准是被主流网络浏览器实现的，而且是可生产网络应用的。借力于这些包含HTML5的技术，数百万的开发者已经能够为Firefox OS编码了。而且为Firefox OS构建的应用通过打包可以轻易被移植到其他平台上，比如使用[Phonegap](http://phonegap.com)。

## 值得HTML5的平台

网络无处不在，在你的电脑，手机，智能电视，甚至是你的视频游戏控制台中。Javascript是全世界最流行的网络编程语言之一。如前所述，当人们谈到HTML5时，他们通常是指HTML、CSS和Javascript这三个技术。HTML的最新进展在与XHTML1.0和HTML4.01比较的范围内带来了一些新特性 - 表单控制、Web Sockets还有更多的语义化标签。而CSS也引入了一些新特性，比如Flexbox和CSS动画，使它更易于创造漂亮的响应式布局。Javascript最新的进展则带来了重要的性能改进和新功能，同时保留了对于初学者和老手一样的易用性。

Firefox OS的本质是一个扩展延伸的移动网络。通过将HTML5作为一等公民，Mozilla已经为数百万的网络开发者开放了其平台。即使一些其他浏览器厂商在他们的移动产品中实践了HTML5，Firefox OS通过提供的一组使用Javascript的可访问底层硬件和系统的API已将其超越。这些API被统称为WebAPI。

## Accessing The Hardware Using The WebAPI

Some earlier platforms also tried to create operating systems that used web technologies for app creation. For example, when the iPhone was introduced to the world, the only way to create apps was using web technologies. However, those web apps were limited in that they had no hardware or device access - meaning that only a limited range of applications could be built. When Apple then allowed developers to code apps in Objective-C, and also access the device's capabilities, it spurred a huge amount of innovation. Sadly, web apps did not gain access to the device's capabilities, and were thus left as "second-class citizens" - this made them unattractive to both users and developers alike, and unable to compete with native apps in that system.

When we say device capabilities we actually mean accessing hardware and OS level features and services: We're talking about things such as updating the address book, sending SMS, and accessing the camera and media gallery. On Firefox OS, the [WebAPI](https://wiki.mozilla.org/WebAPI)s are the means by which you will access many of those capabilities. 

Another earlier platform, WebOS, also offered hardware access via JavaScript but never tried to standardize its APIs. Mozilla is working with the W3C and other stakeholders to make sure that the WebAPIs are an open standard and that other browsers adopt them too. As these APIs are implemented by other browsers, your apps will require less and less changes to work across different platforms.

It's important to emphasize that the WebAPIs are not exclusive to Firefox OS devices. Mozilla is implementing it for the other platforms on which Firefox runs, such as desktop and android. This way, you can use your *open web app* in Firefox OS, Firefox on the desktop and Firefox for Android.

## Freedom to Develop and Distribute

Like everything that Mozilla does, Firefox OS is developed in the open and is free. All development can be followed on [the Mozilla B2G repository](https://github.com/mozilla-b2g/B2G) on GitHub. With Firefox OS you have the freedom to follow and contribute with the development of the system and you also have the freedom to distribute your applications on your own channels or on [The Firefox Marketplace](https://marketplace.firefox.com/). What's really awesome is that all the system applications are written in HTML5, so you can check them out and see how the are put together. 

The main idea is that you're not locked to Mozilla for anything. If you want to pick the source code for the system and change it for your own needs, so be it. If you need to build apps for internal use on your company, or if you want to distribute your creations only on your own web site, you're free to do it. Usually, in other platforms you're locked into the official app store as the only channel to distribute your apps. Firefox OS also has an official market called Firefox Marketplace which has an approval process but you're free to distribute your app outside this store if you want. Like in the web where you can host your web site anywhere you want, on Firefox OS you can do the same with your applications. 

This comes with a small caveat, sadly: some of the WebAPIs are too security sensitive to just allow anyone to use them. To distribute apps that use some of the more "privileged" APIs, you will need to get your applications signed and reviewed by Mozilla's staff. 

## Summary

HTML5 is here to stay and will only get better. Firefox OS is the new open mobile operating system by Mozilla completely based on web technologies. This system is built on the open and offers a robust HTML5 implementation that goes beyond the other platforms by offering the WebAPI which is a collection of APIs to access *hardware and operating system services using JavaScript*. These new APIs are being standardized through the World Wide Web Consortium (W3C) and will hopefully be adopted by other browsers in the future.

In the next chapter we'll quickly get you set up with every you need to develop for Firefox OS. 
