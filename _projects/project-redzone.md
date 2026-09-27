---
title: RedZone Map
description: Safety-Aware GPS Navigation App
image: /images/pages/redzone/car-navigation.jpg
label: Development
featured: true
toc: true
---

Mobile GPS navigation and neighborhood safety application designed to help users avoid high-crime areas by generating safe routing options\
[Zone Technologies Inc.](https://web.archive.org/web/20180321080145/https://www.redzonemap.com/)

RedZone Map was the first GPS navigation app that generated safer routes by combining real-time and historical crime data from police/government sources with live community reports. Available across the US (and Israel in earlier versions), it let users choose between Safe and Risky routes while staying informed about nearby incidents.

## Key features

- Dynamic generation of Safe vs. Risky routes based on crime data and user reports
- Place search with details, reviews, and zone-level safety scores
- Community reporting of suspicious activity, shootings, theft, assaults, vandalism, and police presence (with photo/video upload, likes, and comments)
- Interactive map visualization of “Red Zones” (high-crime areas) and overall safety levels
- Speed-limit monitoring, traffic patterns, and voice-guided turn-by-turn directions
- Proximity alerts for nearby incidents and a safety meter for the current location

The app version supporting Android devices was developed on Android Studio using Java, Room, AWS. Was available for the US.

![iPhone](/images/pages/redzone/redzone-project.png)

<div class="gallery-box">
  <div class="gallery">
    <img src="/images/pages/redzone/car-navigation.jpg" loading="lazy" alt="Project">
    <img src="/images/pages/redzone/raw_pixel_2.jpg" loading="lazy" alt="Project">
    <img src="/images/pages/redzone/s6-duo.jpg" loading="lazy" alt="Project">
  </div>
  <em>Gallery</em>
</div>

## Technical challenges & solutions

This was my first map-based navigation project, so I had to deeply research and implement geospatial algorithms while working within strict API limits:

- Detected whether routes crossed high-risk (“Red Zone”) areas using GeoBounding Boxes, polylines/polygons, ray-casting, and angle-summation point-in-polygon tests
- Applied the Douglas-Peucker algorithm to simplify complex zone boundaries so they stayed under HERE’s coordinate limits (~40 points per banned area) and overall route waypoint limits (~128)
- Calculated travel bearing/orientation, southwest/northeast bounding boxes, and continuously optimized for battery usage during continuous location tracking
- Worked with OSRM for routing and handled real-time data synchronization with community reports

The result was a production Android app that turned multi-source crime and community data into practical, safer navigation for everyday travel.

You can have an overview of the old website in the Web Archive: 

- [2018 Archive](https://web.archive.org/web/20180321080145/https://www.redzonemap.com/)
- [2019 Archive](https://web.archive.org/web/20190530125635/https://www.redzonemap.com/)

<p align="center">

	<iframe src="https://drive.google.com/file/d/1CVPvhrqbyVPPA6CVxwAmUN9OE2SHwthx/preview" 		width="640" height="360" allow="autoplay"></iframe>

  <img src="/images/pages/redzone/redzone-3-screens.png">
  <small>Images credit: <a href="https://meltedcolor.com">Anthony García</a></small>

</p>



