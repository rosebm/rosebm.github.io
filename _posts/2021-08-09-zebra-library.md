---
layout: post
title: 'Printing Text and Images on Zebra.'
tags: [Android, Zebra, Java]
featured_image_thumbnail: assets/images/posts/2021/zebra-printer-2_thumbnail2.jpg
featured_image: assets/images/posts/2021/zebra-printer-2.png
featured: true
hidden: true
---

Struggling to understand CPCL and print receipts on Android?. This basic project helps to connect, via bluetooth, to a [Zebra's](https://www.zebra.com/us/en/products/printers.html) printer and emit a text and an image. 

<!--more-->

When I was brand new on the Android world, I got my first work challenge: to print the receipts with the Zebra's printers; as with any receipt, I needed to print the company's logo and all products bought with their prices and total. These printers are compact and easy to carry, ideal for receipts, barcodes, labels, etc.

It took me a while to understand the manuals, the language and how to work with margins and format text on the printers; this is the reason why I decided to create a project, so I could publish it and give access to anyone else that was struggling just like me. 

The project consists of a library and a simple app that consumes it by printing with CPCL language, and it was tested on a Zebra RW420. Currently is not maintained because I don't have access anymore to the Zebra printers; nevertheless, I hope it helps you to create a base.

At the end I was able to print a more formatted receipt, like this:

![receipt](assets/images/posts/2021/zebra-printer-pdf.png "Receipt")

You can find the code here [GitHub](https://github.com/rosebm/ZebraPrintingAndroid)
