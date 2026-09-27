---
layout: post
title: 'Detecting Risky Zones on a Route: Lessons from Old Notes'
description: Old notes on detecting when a route polyline crosses a risk polygon..
date: 2026-09-26 15:01:35 +0300
tags: [Navigation, Algorithms, Maps, Android]
featured_image_thumbnail: assets/images/posts/2025/kotlin_to_flutter/android-iphone.png
image: /images/posts/2026/lessons_from_navigation_map/route-crossing-redzone.png
toc: true
---

<!--more-->

# Detecting Risky Zones on a Route: Lessons from Old Notes

While cleaning at home the other day, I found a stack of old notebook pages filled with sketches and scribbled ideas from years ago. They belonged to a navigation project where the goal was to generate safer routes by avoiding high-crime areas. Looking through them again made me realize how much manual research and trial-and-error went into something that, with today’s AI tools, would probably be solved far more quickly. Still, the core concepts remain interesting, so I decided to share them.

## Starting with GeoBounding Boxes

The process began with the fastest route returned by the routing engine. I extracted its bounding latitude and longitude values and used them to request nearby Red Zones from the backend. Those zones were then treated as banned areas so the engine could apply a dynamic penalty and return a safer alternative.

The real question came next: once a route polyline arrived from HERE, how could I reliably detect whether it crossed a Red Zone?

A typical sketch looked like this: a large rectangle representing the overall map bounds, a diagonal line running from point A to point B (the route), and a shaded irregular polygon sitting in the middle that the line cut through. The problem was obvious—polylines often contained more than a thousand points, so a naïve check would be too slow.

## Orientation and Bounding Boxes

To keep things efficient I first calculated the orientation of travel (bearing) for each segment and built a tight bounding box around it. The box was defined by its southwest (SW) and northeast (NE) corners:

- SW = minimum latitude + minimum longitude
- NE = maximum latitude + maximum longitude

![iPhone](/images/posts/2026/lessons_from_navigation_map/heading-and-bounding-box.png)
Bounding Box

I also had to understand the different “norths”:

- True North – the Earth’s rotational axis; meridians of longitude converge there.
- Magnetic North – the direction a compass needle points.
- Grid North – the direction of the map’s grid lines (usually close enough to True North that it can be ignored for most navigation).

The angular difference between True North and Magnetic North is called declination. Map bearing itself is simply the horizontal angle measured clockwise from north to a given direction (for example 45° or 320°).

When the route segment ran almost parallel to one axis, the bounding box needed to be kept thin; otherwise the overlap test became too loose.

![iPhone](/images/posts/2026/lessons_from_navigation_map/declination.png)
Declination

![iPhone](/images/posts/2026/lessons_from_navigation_map/map-bearing.png)
Map Bearing

## Precise Intersection Tests

After the quick bounding-box filter, more accurate algorithms were required:

- Ray-casting
- Winding-number
- Angle summation

These answered the fundamental geometric question: does a given point (or line segment) lie inside the Red Zone polygon?

## Practical Constraints

HERE imposed strict limits—only about 40 coordinates were allowed for a banned area—so I had to run a polyline simplification algorithm (Douglas-Peucker style) before sending the zones. There was also a 128-waypoint limit on routes that was expected to be raised in later releases.

![iPhone](/images/posts/2026/lessons_from_navigation_map/polyline-points-in-polygon.png)
Polyline points in a Polygon

Because the same routes needed to appear on both Android and iOS, the HERE API calls were moved to the backend. Traffic data, speed limits and voice guidance were layered on top of the geometric checks.

Density of risk was sometimes estimated by counting the number of incident points that fell inside a search radius, giving a quick visual sense of how “hot” an area was.

![iPhone](/images/posts/2026/lessons_from_navigation_map/radius-search-and-bbox-cases.png)
Density based on the number of points inside the circle

All of these pieces—bearing calculations, carefully constructed SW/NE boxes, polygon simplification, and classic point-in-polygon tests—worked together so the app could decide, in real time, whether a route should be labelled risky or safe.

That’s it! Going through those old notes was a reminder of how much pure geometry and careful constraint handling went into something that felt straightforward on the surface. It was overwhelming at the time getting all the pieces to fit, but looking back it was also a lot of fun—and definitely something I’m glad I dug into!. 🚀




