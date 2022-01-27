---
layout: post
title: 'Printing Text and Images on Zebra.'
tags: [Android, Zebra, Java]
featured_image_thumbnail: assets/images/posts/2021/zebra-printer-2_thumbnail2.jpg
featured_image: assets/images/posts/2021/zebra-printer-2.png
---

Basic project that helps to connect, via bluetooth, to a [Zebra's](https://www.zebra.com/us/en/products/printers.html) printer and emit a text and an image. 

<!--more-->

When I was brand new on the Android world, I got my first work challenge: to print the receipts with the Zebra's printers, and emit a text and an image. These are compact and easy to carry, ideal for receipts, barcodes, labels, etc.

This is a library I created long time ago, when I just started learning Android. It took time to understand Zebra manuals, reason I decided to publish it in case anyone else was struggling just like me. 

The project consists of a library and a simple app that consumes it to print with CPCL language, and it was tested on a Zebra RW420. Currently is not maintained because I don't have access anymore to the Zebra printers; nevertheless, I hope it helps you to create a base. 

You can find the code here [GitHub](https://github.com/rosebm/ZebraPrintingAndroid)
