---
title: "Registering Magic User Interface"
date: 2021-12-10T19:51:00-06:00
---

{{< figure src="mui-registered.png" title="My registered copy of Magic User Interface, at the legendary Commodore HQ." >}}

[Magic User Interface](http://www.sasg.com/mui/) (MUI) is a GUI toolkit for the Amiga which saw widespread use throughout the mid-90s and 2000s. It improved on GadTools with automatic layout, visual customization, keyboard control, context-sensitive help, and a large set of extensible GUI components.

MUI was met with mixed reception. Some criticized its poor performance on lower end Amigas. Others felt it didn't integrate well with existing applications. Many agreed it was the most powerful GUI toolkit available, however, and a large ecosystem of applications and utilities grew around it.

I was very fond of MUI and wrote some of my earliest applications with it. One thing I regret is not registering my copy with the developer. MUI is shareware. A registered copy unlocks many customization options, including the very nice XEN theme (pictured above). This theme integrates with the very popular [MagicWB](http://www.sasg.com/mwb/) project.

This year I discovered that you can still [register MUI](http://www.sasg.com/cgi-sasg/order_info?app=mui), via Paypal! Unfortunately the author lost access to his key registration program. From what I've read only a generic key has been received by users over the last decade. This doesn't have your customized name and address displayed in the MUI preferences program.

I didn't even receive that key when I registered my copy. Honestly, I was surprised the link worked at all! Not to be deterred I found an [interesting discussion](https://eab.abime.net/showthread.php?t=37249&page=5) on the EAB forum. Some users had reverse engineered the key verification algorithm in muimaster.library. Even more interestingly, they'd discovered the key generation algorithm in the publicly distributed library!

The post was vague on a few important details but I saw a fun challenge here. I analyzed the key verification algorithm by stepping through with the UAE debugger. Combined with observations from the discussion thread I was able to recreate the key generation process. I then built a keygen program to automate it.

{{< figure src="mui-keygen.png" title="My very own MUI key generator. I couldn't resist using MUI for the GUI. Oh, the sweet irony!" >}}

For obvious reasons I'm not going to distribute the program or its source code. In fact I encourage you to [register MUI](http://www.sasg.com/cgi-sasg/order_info?app=mui) if you enjoyed using it as much as I did (and still do!). Stefan contributed a lot to the Amiga community. The EAB [discussion thread](https://eab.abime.net/showthread.php?t=37249&page=5) provides a way to get your own personalized key with proof of registration.
